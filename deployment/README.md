# Deploying Tavern-Danswer
Revisit README.old for original deployment directions.

## Set up venv with uv

   1. In terminal `pip install uv`

   2. In terminal `uv venv --python 3.11`
      - python 3.11 is recommended by the developers, 3.12 doesn't work with tensorflow. I'm unsure about older versions.

   3. In terminal `.\.venv\Scripts\activate`

## Set up .env

   1. Copy .\deployment\docker_compose\env.prod.template and rename copy to .env

   2. Set `AUTH_TYPE=disabled` for local development. If using this configuration for cloud deployment, set to google_oauth and use GCP to get client_id and client_secret.

   3. Delete or comment out `SECRET=` (& the Google OAuth variables if you're not using them)

   4. Uncomment `GEN_AI_API_KEY=` and set it to your OpenAI key.

   5. Add `DISABLE_TELEMETRY=True`

## Docker Compose:

0. Ensure Docker Engine is running.

1. Run `docker compose -f .\deployment\docker_compose\docker-compose.dev.yml -p danswer-stack up -d --build`
   - optionally add --force-recreate (if you don't want containers to be reused)
   - optionally replace --build with --pull always (if you don't want to rebuild from source)

2. To shut down the deployment, run:
   - To stop the containers: `docker compose -f .\deployment\docker_compose\docker-compose.dev.yml -p danswer-stack stop`
   - To delete the containers: `docker compose -f .\deployment\docker_compose\docker-compose.dev.yml -p danswer-stack down`

3. To completely remove Danswer run:
   - **WARNING, this will also erase your indexed data and users**
   - `docker compose -f .\deployment\docker_compose\docker-compose.dev.yml -p danswer-stack down -v`

## For development

1. Run the following as relevant:
      `uv pip install -r .\backend\requirements\cdk.txt`
      `uv pip install -r .\backend\requirements\default.txt`
      `uv pip install -r .\backend\requirements\dev.txt`
      `uv pip install -r .\backend\requirements\model_server.txt`