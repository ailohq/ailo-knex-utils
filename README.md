# @ailo/knex-utils

Utilities to be used in node.js repos using `knex`.

## Usage

First, add it as a dependency:

```sh
yarn add @ailo/knex-utils
```

Then, depending on what you need:

#### Test Utils (e.g. `useKnex`)

```ts
// database/migrations/tests/1002_rename_user_to_person.test.ts
import { useKnex } from "@ailo/knex-utils/build/main/test-utils";
import knexFactory from "knex";
import { testMigrationConfig } from "knexConfig";
import moment from "moment";
import { up } from "../1002_rename_user_to_person";

const knex = useKnex({
  knex: knexFactory(testMigrationConfig),
  knexConfig: testMigrationConfig,
  migrateTo: "1001",
});

it("db migration 1002_rename_user_to_person works", async () => {
  await knex("user").insert({
    name: "john",
  });
  await up(knex);

  const [user] = await knex("person").select("id", "name");
  expect(user.name).toEqual("john");
});
```

#### `local-db` shell file

```sh
yarn local-db
yarn local-db test
```

## Development

```sh
yarn
yarn start
```

## Testing

```sh
yarn lint # prettier and eslint
yarn test # unit tests
yarn test:watch # unit tests in watch mode
```

## Releasing

```sh
yarn release
```
