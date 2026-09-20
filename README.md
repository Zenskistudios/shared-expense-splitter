# Shared expense splitter

A group shares costs and stops arguing about them. Somebody logs what
they paid, the app works out who owes whom, and settling up clears it.

No money moves through this product. It records that somebody paid
somebody else, which means nothing in it waits on a payment provider,
and it also means the record is the entire value of the thing. If the
numbers are wrong, there is nothing else here.

## What finished looks like

A stranger can open the URL, register, start a group, invite the
people in it, add an expense and see at once who owes whom, split a
bill the way it was actually split, correct a mistake weeks later
without the numbers going wrong, and settle up in the fewest payments
that clear the group.

Finished does not mean running on a laptop. It means the four people
who went on the trip can all use it, on their own phones, on the
internet.

## The road map

Eight sprints. Each one makes a different part of that sentence true,
and the order is not arbitrary: take any sprint out and the sentence
stops being true.

| # | Sprint | What it makes possible |
|---|---|---|
| 1 | Getting in | Somebody can make an account and come back to it |
| 2 | A group of people | A group with the right people in it, visible to nobody else |
| 3 | What one person paid | One payer, an equal split, and every penny accounted for |
| 4 | Splits that match what happened | Shares, exact amounts, and paying for something you are not part of |
| 5 | Settling up | A payment clears the right pair, and the group settles in the fewest payments |
| 6 | Nothing is deleted | Corrections, removals and somebody leaving while they still owe |
| 7 | The people are real | Invitations that only the right person can use, inheriting what is already owed |
| 8 | Put it where people are | It is on the internet, at its own address, and the history survives losing the machine |

## The hard part

Every expense adds up to zero across the group. What one person paid,
the others owe in shares, and the sum of everybody's position is
exactly nothing. That claim has to survive a bill of £10 split three
ways, a split by exact amounts, somebody paying for a thing they are
not part of, a settlement between two people that must not touch
anybody else, somebody leaving while they still owe, and an expense
edited three weeks and forty expenses later.

The last one is why this is a ledger rather than a balance column.
Sprint six is where that is proved, and sprint three is where the
habit that makes it possible is formed.

## Working on it

The sprints, their briefs and the tickets under them are in
Blacksmith. Start with sprint one; each sprint opens as the one before
it closes.

The reading attached to a sprint is worth opening before its first
ticket rather than after. It carries the parts a ticket deliberately
does not: what the alternatives were, and which one you are choosing
between.

## What this is built with

The whole product: an API and the screens that use it.

- **Django** with **Django REST Framework** for the API.
- **SimpleJWT** for authentication, against a custom user model in `apps/users`.
- **drf-spectacular** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, database, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the secret key, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── config/
│   ├── settings/        # base, development, production
│   └── urls.py          # where routes are mounted
├── apps/
│   └── users/           # the custom user model, and auth
├── utils/               # shared helpers, base model
├── manage.py
└── requirements.txt

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a serializer or a route. |
| `blacksmith make:resource Post` | Scaffold a model, serializer, viewset and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run a Django management command, e.g. `blacksmith backend createsuperuser`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Django and React project. Nothing here is a dependency on us. |
