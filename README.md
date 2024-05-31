This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/basic-features/font-optimization) to automatically optimize and load Inter, a custom Google Font.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.

## Run Keycloak server
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin -e KC_PROXY=edge quay.io/keycloak/keycloak:24.0.4 start-dev

docker run -p 8080:8080 -v keycloak:/opt/keycloak/data/h2 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin -e KC_PROXY=edge quay.io/keycloak/keycloak:24.0.4 start-dev


## How to use Keycloak + API Gateway

https://apisix.apache.org/blog/2022/07/06/use-keycloak-with-api-gateway-to-secure-apis/


## How to implement docker in Keycloak

Create a docker volume for the H2 database files
docker volume create keycloak
Set the correct permissions on the new volume (the keycloak user in the quay.io/keycloak/keycloak:18.0.2 image container is UID: 1000, GID: 0)
docker run \
    --rm \
    --entrypoint chown \
    -v keycloak:/keycloak \
    alpine -R 1000:0 /keycloak
Use the docker volume to persist the H2 database
docker run \
    -p 8080:8080 \
    -e KEYCLOAK_ADMIN=admin \
    -e KEYCLOAK_ADMIN_PASSWORD=admin \
    -v keycloak:/opt/keycloak/data/h2 \
    quay.io/keycloak/keycloak:18.0.2 start-dev --http-relative-path /auth