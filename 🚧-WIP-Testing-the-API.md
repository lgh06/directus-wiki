## Testing schema
Tests Location: `/api/src/tests/`

Mock Location: According to Jest documentation mocks must go in a folder called __mocks__ in the same directory as the file you _want to mock_, not the test file.

## Mocking the Database and Knex
There are two ways to mock the database and Knex, Jest.fn() and the package `knex-mock-client`.

 You can mock the database and knex with the `knex-mock-client` package. `knex-mock-client` allows you to test your queries and provide a response. It should be used for validating queries. It also comes in handy when mocking getDatabase() and other modules as it can be combined with Jest.fn() for a deep mock.

Example of mocking getDatabase()
```ts
import getDatabase from '../../../src/database/index';

jest.mock('getDatabase', () => {
    return knex({ client: MockClient });
});
someFunctionThatUsesGetDatabaseOnce()

expect(getDatabase()).toBeCalledTimes(1)
```

Example of deep mock using `database/migrations/run` 
```ts
describe('when passed the argument up', () => {
        it('returns "Nothing To Updage" if no directus_migrations', async () => {
            tracker.on.select('directus_migrations').response(['Empty']);
            await run(db, 'up').catch((e: Error) => {
                expect(e).toBeInstanceOf(Error);
                expect(e.message).toBe('Nothing to upgrade');
            });
        });
    });

```

See  [knex-mock-client  -  npm](https://www.npmjs.com/package/knex-mock-client) for full documentation and usage examples