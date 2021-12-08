 ## Set up the testing file:
	-  Folder Structure:
	Currently, the structure for the Items endpoint is based on the relationships being tested. This is being reorganized to have files based on actions, such as createOne. Going forward use the new structure and the old tests will be reorganized.
	    - Put tests in `./tests/e2e/api/<YOUR ENDPOINT>/`. Name files after the action.
	    - Example: `./tests/e2e/items/createOne.test.ts`

## Inside the testing file:
The first `describe` is the service name (auth, items, collections, etc). The second describe is the endpoint or action. The third describe is for the scenario
```ts
describe('auth' () => {
    describe('login', () => {
        describe('when correct credentials are provided', () => {})
    })
})
```
Use "it" to describe the result
Use it.each(getDBsToTest()) to test on every enabled database. getDBsToTest() must be imported
"vendor" is the DB returned from getDBsToTest(). It is used to replace %p in the test description
Start the test description with %p so that which DB failed is obvious, this is more important when running tests locally.


## Full example: 
`tests/e2e/api/auth/login.test.ts`
```ts
describe('auth', () => {
	describe('login', () => {
		const databases = new Map<string, Knex>();

		beforeAll(async () => {
			const vendors = getDBsToTest();

			for (const vendor of vendors) {
				databases.set(vendor, knex(config.knexConfig[vendor]!));
			}
		});

		afterAll(async () => {
			for (const [_vendor, connection] of databases) {
				await connection.destroy();
			}
		});

		describe('when correct credentials are provided', () => {
			it.each(getDBsToTest())(`%p returns an access_token, expires and a refresh_token for admin`, async (vendor) => {
				
				const url = `http://localhost:${config.ports[vendor]!}`;
				const response = await request(url)
					.post(`/auth/login`)
					.send({ email: 'test@admin.com', password: 'TestAdminPassword' })
					.expect('Content-Type', /application\/json/)
					.expect(200);
				expect(response.body).toMatchObject({
					data: {
						access_token: expect.any(String),
						expires: expect.any(Number),
						refresh_token: expect.any(String),
					},
				});
			});
		});
		describe('when incorrect credentials are provided', () => {
			it.each(getDBsToTest())(`%p returns code: UNAUTHORIZED for incorrect password`, async (vendor) => {
				const url = `http://localhost:${config.ports[vendor]!}`;

				const response = await request(url)
					.post(`/auth/login`)
					.send({
						email: 'test@admin.com',
						password: 'IncorrectPassword',
					})
					.expect('Content-Type', /application\/json/)
					.expect(401);
				expect(response.body).toStrictEqual({
					errors: [
						{
							message: 'Invalid user credentials.',
							extensions: {
								code: 'INVALID_CREDENTIALS',
							},
						},
					],
				});
			});
		});
	});
});

```