# Fly.io and Global Flagsmith

## Set Up

```bash
# Create a postgres database in LHR
fly pg create --name flagsmith-postgres --region lhr

# Create a postgres read replica in LAX
fly volumes create pg_data -a flagsmith-postgres --size 10 --region lax

# Scale up postgres for LAX inclusion
fly scale count 3 -a flagsmith-postgres

# Create the API
fly apps create flagsmith

# Attach the DB
fly pg attach --postgres-app flagsmith-postgres

# Pin 2 API instances to our two regions LHR and LAX
fly volumes create flagsmith_data --region lhr --size 10
fly volumes create flagsmith_data --region lax --size 10

# Deploy the front end
fly deploy

# Scale up the API so that it deploys to LHR and LAX
fly scale count 2

# API needs more RAM
fly scale vm shared-cpu-1x --memory=1024
```
