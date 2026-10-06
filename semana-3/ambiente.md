# Ambiente — versões-alvo (F0-03) e situação da máquina

Fonte: definição do time + branch `gmoraes_docker` do `elo_api` (verificado em 06/10/2026).

| Tecnologia | Alvo | Neste Docker (gmoraes_docker) | Minha máquina |
|---|---|---|---|
| PostgreSQL | 18.6 | `postgres:18.6-alpine` ✅ | psql 16.15 (cliente) ❌ |
| Ruby | 4.0.x | ainda não (placeholder nginx) | 3.2.2 (rbenv tem 4.0.5 disponível) ❌ |
| Rails | 8.1.4 | ainda não (entra na F0-04) | 7.0.4 ❌ |
| Node.js | 24.21.0 | ainda não | não instalado ❌ |
| Angular | 22 | — (fica no `elo_app`) | — |
| TypeScript | 6.0.x | — (fica no `elo_app`) | — |

O container `api` hoje é um **nginx placeholder**; só o PostgreSQL 18.6 cumpre a matriz. Ruby/Rails/Node serão configurados na F0-04.

## Instalado na máquina (06/10/2026)
| Item | Versão | Como |
|---|---|---|
| Ruby | 4.0.5 | `rbenv install 4.0.5` (global continua 3.2.2, para não quebrar projetos antigos) |
| Rails | 8.1.4 | `gem install rails -v 8.1.4` (só no Ruby 4.0.5) |
| Node.js | 24.21.0 (npm 11.19.0) | nvm 0.40.8, alias `default` |
| Angular CLI | 22.2.1 | `npm i -g @angular/cli@22.2.1` |
| TypeScript | 6.0.3 | `npm i -g typescript@6.0.3` |
| PostgreSQL | 18.6 | via Docker (`./bin/docker-up -d db`, porta 5433); cliente `psql` local é 16.15 |

Usar o Ruby 4.0.5 num projeto: `rbenv local 4.0.5` (ou o `.ruby-version` que a F0-04 vai criar). Em shell avulso: `RBENV_VERSION=4.0.5 ruby -v`.
Node: abrir um terminal novo (nvm foi adicionado ao `.bashrc`) ou `source ~/.nvm/nvm.sh`.

Atenção: porta 8080 (Keycloak, perfil `platform`) conflita com o container `godc_php55_theia`; pare-o se for subir esse perfil.

## Validação do `elo_api` (branch `desenvolvimento`, 06/10/2026)
- Ruby 4.0.7 (`.ruby-version` do projeto) + Rails 8.1.4 via `bundle install`: ok.
- PostgreSQL 18.6 + papéis (`script/f0-06_bootstrap.sh`) + `db:migrate` + **rspec: 22 exemplos, 0 falhas**.
- Variáveis usadas: `POSTGRES_PORT`, `POSTGRES_SUPERUSER=elo`, `POSTGRES_SUPERUSER_PASSWORD=elo` (valores de dev do compose).

Problemas encontrados no ambiente:
1. **Rede Docker bridge bloqueada no host**: o host não alcança containers (nem pela porta publicada) e containers em rede bridge não saem para a internet. Provável causa: firewall/iptables. Contorno: Postgres de teste com `--network host` na porta 5434 (`elo-pg-teste`). Correção definitiva exige root (ex.: reiniciar o Docker).
2. **`POSTGRES_DB=elo_development` no compose** cria o banco com dono `elo`; o bootstrap espera criá-lo com dono `elo_owner`, e o `db:migrate` falha com "permission denied". Contorno: `ALTER DATABASE elo_development OWNER TO elo_owner`.
3. **`pg_dump` do host é 16** e o servidor é 18.6: o `db:migrate` falha no dump do `structure.sql`. Precisa de `postgresql-client-18`.
   - Resolvido: `postgresql-client-18` instalado via repositório PGDG (`pg_dump` 18.6 no PATH).
   - Efeito colateral: o `db:migrate` agora **regenera `db/structure.sql` de forma diferente** do arquivo versionado (inclui `CREATE SCHEMA public` e `elo.schema_migrations`) e isso quebra o `db:test:load_schema`. Reverter com `git checkout -- db/structure.sql` após migrar, e não commitar o dump regenerado sem alinhar com o time.
