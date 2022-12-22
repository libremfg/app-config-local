# Libre 3 - Local Docker-compose configuration
This repository contains a local server that runs in docker-compose.

It runs the following components:
- Keycloak: An Identity Management server that can issue OAuth2 tokens that are required to access libreBaas
- LibreBaas Zero: The management instance of the distributed database cluster
- LibreBaas Alpha: A worker node in the distributed database cluster that serves the GraphQL API

Some initial data files for Keycloak and LibreBaas are also contained in the repo

To run the repo, navigate to the root of the project and run ``` docker-compose up```
It will take a minute or so for Keycloak to start up, and while that is happening, the LibreBaas Alpha node will restart in a loop as it requires keycloak to be running.
When Keycloak is running, the Alpha will run.

The database will have the Libre3 schema in it already, but it can be updated with later versions of the schema by posting them into the /admin/schema endpoint.
Follow the documentation at libremfg.github.io

## Ports
- Keycloak runs on 8090
- Zero uses 5080 and 6080
- Alpha uses 8080 and 9080

## Keycloak

The admin user name and password have been set to 
- Username admin
- Password admin

A user has been set up in Keycloak for admin@libremfg.com with a password of admin
This user has full access

## Accessing the GraphQL API with GraphQL Playground.

The LibreBaas database and GraphQL API are secured with OAuth2, and require an Authorization 
Bearer token.

You can get a token using the following curl command
```bash
curl --location --request POST 'http://localhost:8090/realms/libre/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'username=admin@libremfg.com' \
--data-urlencode 'password=admin' \
--data-urlencode 'client_id=libreUI' \
--data-urlencode 'client_secret=FItUacME3NkdlOg90T1DhjpljU6KElT9'
```

that will provide a response similar to the following:
```graphql
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJBcmg5VlNDR0VvcFB2Nnc1b096NENxc0o4WHd2cEpQLUxxNFExTXJERTdBIn0.eyJleHAiOjE2NzE3MDg1MDMsImlhdCI6MTY3MTY3MjUwMywianRpIjoiOTViMWE1ZjQtM2JjYS00YzQ2LTlmNWMtMGQ0MjEzMmEzNmEzIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDkwL3JlYWxtcy9saWJyZSIsImF1ZCI6WyJsaWJyZVVJIiwibGlicmVCYWFzIiwiYWNjb3VudCJdLCJzdWIiOiJiZWRjYzZlOS1iNTE1LTQ2ODYtYTUyNS01NDZjZWZlMGQ0MDIiLCJ0eXAiOiJCZWFyZXIiLCJhenAiOiJsaWJyZVVJIiwic2Vzc2lvbl9zdGF0ZSI6ImJmMjg1OGMzLTc4OGUtNDVjOS04MjEyLThkMDlhZjVmNTM4NCIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJBRE1JTiIsInVtYV9hdXRob3JpemF0aW9uIiwiZGVmYXVsdC1yb2xlcy1saWJyZSIsImxpYnJlUmVhbG1Sb2xlIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJlbWFpbCBwcm9maWxlIGxpYnJlQ2xpZW50U2NvcGUiLCJzaWQiOiJiZjI4NThjMy03ODhlLTQ1YzktODIxMi04ZDA5YWY1ZjUzODQiLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwibmFtZSI6IkFkbWluIExpYnJlIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiYWRtaW5AbGlicmVtZmcuY29tIiwiZ2l2ZW5fbmFtZSI6IkFkbWluIiwiZmFtaWx5X25hbWUiOiJMaWJyZSIsImVtYWlsIjoiYWRtaW5AbGlicmVtZmcuY29tIn0.Ym6Z4viSI3FNeelYmf3NwNrg6qXWXJ9PQ0VDP73Pz93R79ge7-GjiGXmGsLaAUrQy7L4JeFddidRR0EF75dDwMxBrR5CLXjk1ZjcMJIdGvcruM9qPDuqKazvmclVFNWEU8gOsRSesTkqAaBnqifR3vi-V5U994L6kQHm2DhpU04DV6FB53Z7tk0YKB_hv-f6oOvU4nHs8z46rU-nUzI7NezCefmT2UW4bT_x2nF8g1s6XJbLV1HPqvVlwJD1pRn2OVANjCYycr1ewNiIVFkJdPXbjefSgT3hbVjZxvWxa6l5txpG1E9LH6q0dZpciJcRt1YMCK0E4dVZ9Q8fLKphhQ",
    "expires_in": 36000,
    "refresh_expires_in": 1800,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyZDNhODUzYi0xODI4LTQxMTgtODA3Ny04ZTA5ODlhNGE1NjUifQ.eyJleHAiOjE2NzE2NzQzMDMsImlhdCI6MTY3MTY3MjUwMywianRpIjoiZWI5ODhiZTQtZWRhYS00OTRhLTg4NWItZGQxNTM5MGMzYWNlIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDkwL3JlYWxtcy9saWJyZSIsImF1ZCI6Imh0dHA6Ly9sb2NhbGhvc3Q6ODA5MC9yZWFsbXMvbGlicmUiLCJzdWIiOiJiZWRjYzZlOS1iNTE1LTQ2ODYtYTUyNS01NDZjZWZlMGQ0MDIiLCJ0eXAiOiJSZWZyZXNoIiwiYXpwIjoibGlicmVVSSIsInNlc3Npb25fc3RhdGUiOiJiZjI4NThjMy03ODhlLTQ1YzktODIxMi04ZDA5YWY1ZjUzODQiLCJzY29wZSI6ImVtYWlsIHByb2ZpbGUgbGlicmVDbGllbnRTY29wZSIsInNpZCI6ImJmMjg1OGMzLTc4OGUtNDVjOS04MjEyLThkMDlhZjVmNTM4NCJ9.irtxlsMzeuO1Jvdmof5-Dn1qE81i7W_zViM-_mPkIcI",
    "token_type": "Bearer",
    "not-before-policy": 0,
    "session_state": "bf2858c3-788e-45c9-8212-8d09af5f5384",
    "scope": "email profile libreClientScope"
}
```

Copy the value of the access_token.

When using GraphQL Playground, add a header as follows replacing <access_token> with the value from above
```graphql
{"Authorization": "Bearer <access_token>"
```

