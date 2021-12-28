# @anatine/zod-mock

Generates a mock data object using [faker.js](https://www.npmjs.com/package/faker) from a [Zod](https://github.com/colinhacks/zod) schema.

----

## Installation

Both openapi3-ts and zod are peer dependencies instead of dependant packages.
While `zod` is necessary for operation, `openapi3-ts` is for type-casting.

```shell
npm install faker zod @anatine/zod-mock
```

----

## Usage

### Take any Zod schema and create mock data

```typescript
import { generateMock } from '@anatine/zod-mock';

const schema = z.object({
      uid: z.string().nonempty(),
      theme: z.enum([`light`, `dark`]),
      email: z.string().email().optional(),
      phoneNumber: z.string().min(10).optional(),
      avatar: z.string().url().optional(),
      jobTitle: z.string().optional(),
      otherUserEmails: z.array(z.string().email()),
      stringArrays: z.array(z.string()),
      stringLength: z.string().transform((val) => val.length),
      numberCount: z.number().transform((item) => `total value = ${item}`),
      age: z.number().min(18).max(120),
    });

const mockData = generateMock(schema);

// ...
```

This will generate mock data similar to:
```json
{
  "uid": "3f46b40e-95ed-43d0-9165-0b8730de8d14",
  "theme": "light",
  "email": "Alexandre99@hotmail.com",
  "phoneNumber": "1-665-405-2226",
  "avatar": "https://cdn.fakercloud.com/avatars/olaolusoga_128.jpg",
  "jobTitle": "Lead Brand Facilitator",
  "otherUserEmails": [
    "Wyman58@example.net",
    "Ignacio_Nader@example.org",
    "Jorge_Bradtke@example.org",
    "Elena.Torphy33@example.org",
    "Kelli_Bartoletti@example.com"
  ],
  "stringArrays": [
    "quisquam",
    "corrupti",
    "atque",
    "sunt",
    "voluptatem"
  ],
  "stringLength": 4,
  "numberCount": "total value = 25430",
  "age": 110
}
```

----
## Behind the Scenes

**`zod-mock`** tries to generate mock data from two sources.

- ### Object key name ie(`{ firstName: z.string() }`)
  This will check the string name of the key against all the available `faker` function names.
  Upon a match, it uses that function to generate a mock value.
- ### Zodtype ie(`const something =  z.string()`)
  In the case there is no key name (the schema doesn't contain an object) or there is no key name match,
  `zod-mock` will use the primitive type provided by `zod`.

  Some zod filter types (email, uuid, url) and number's (min, max) will also modify the results.

----

## Missing Features

- String length constraints are not taken into account
- No pattern for passing options into `faker`, such as setting phone number formatting

----
## Credits

- ### [express-zod-api](https://github.com/RobinTail/express-zod-api)
  A great lib that provided some insights on dealing with various zod types.

----

This library is part of a nx monorepo [@anatine/zod-plugins](https://github.com/anatine/zod-plugins).