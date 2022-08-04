# style_guide

```

xxxxxxx
├── README.md
├── analysis
├── data
├── dbt_project.yml
├── macros
├── models
│   ├── intermediate
│   │   └── finance
│   │       ├── _int_finance__models.yml
│   │       └── int_payments_pivoted_to_orders.sql
│   ├── marts
│   │   ├── finance
│   │   │   ├── _finance__models.yml
│   │   │   ├── orders.sql
│   │   │   └── payments.sql
│   │   └── marketing
│   │       ├── _marketing__models.yml
│   │       └── customers.sql
│   └── staging
│      ├── sap
│      │   ├── _sap__docs.md
│      │   ├── _sap__models.yml
│      │   ├── _sap__sources.yml
│      │   ├── base
│      │   │   ├── base_sap__customers.sql
│      │   │   └── base_sap__deleted_customers.sql
│      │   ├── stg_sap__customers.sql
│      │   └── stg_sap__orders.sql
│      └── mongodb
│          ├── _mongbo__models.yml
│          ├── _mongbo__sources.yml
│          └── stg_mongbo__payments.sql
│  
├── packages.yml
├── snapshots
└── tests
```
NOTE: All data sources and models must be documented from the very begining of development. This will help us achieve an optimal data culture.

Data documentation is the dolution not something that results from the solution.
## Models
- All model names must be unique
- Staging models must be of the form ```stg_[source]__[entity]s.sql```
- Staging models are the only models that reference source
- Base models must be of the form ```base_[source]__[entity]s.sql```
- Intermediary models should be of the form ```int_[entity]s_[verb]s.sql```

## CTEs
- All staging models must be organized into CTEs: one pulling in a source table via the source macro and the others applying your transformations.
- All {{ ref('...') }} statements should be placed in CTEs at the top of the file
- CTE names should be as verbose as needed to convey what they do
- CTEs with confusing or noteable logic should be commented
- CTEs that are duplicated across models should be pulled out into their own models
- create a final or similar CTE that you select from as your last line of code. 

## Yml files

- Yml files must be of the format ```_[directory]__models.yml```
- There must be a ```_[directory]__models.yml``` per directory in the models folder that configures all the models in that directory. For staging folders, also include a ```_[directory]__sources.yml``` per directory.
- If you utilize doc blocks in your project, create a ```_[directory]__docs.md``` markdown file per directory containing all your doc blocks for that folder of models.

## Testing
- Every subdirectory should contain a .yml file, in which each model in the subdirectory is tested.
- At a minimum, unique and not_null tests should be applied to the primary key of each model.

## Materializations
- Staging models should be materialized at views
- Intermediary models should be materialized as views in a custom schema with special permissions.
- Mart models should always be materialized as tables

## Naming and field conventions

- Schema, table and column names should be in snake_case.
- Use names based on the business terminology, rather than the source terminology.
- Each model should have a primary key.
- The primary key of a model should be named ```<object>_id``` e.g. account_id – this makes it easier to know what id is being referenced in downstream joined models.
- For base/staging models, fields should be ordered in categories, where identifiers are first and timestamps are at the end.
- Timestamp columns should be named ```<event>_at```. e.g. ```created_at```, and should be in UTC. If a different timezone is being used, this should be indicated with a suffix, e.g ```created_at_pt```
- Booleans should be prefixed with ```is_``` or ```has_.```
- Price/revenue fields should be in decimal currency (e.g. 19.99 for $19.99; many app databases store prices as integers in cents). If non-decimal currency is used, indicate this with suffix, e.g. price_in_cents.
- Avoid reserved words as column names
- Consistency is key! Use the same field names across models where possible, e.g. a key to the customers table should be named customer_id rather than user_id. 

## Jinja

- When using Jinja delimiters, use spaces on the inside of your delimiter, like {{ this }} instead of {{this}}
- Use newlines to visually indicate logical blocks of Jinja



