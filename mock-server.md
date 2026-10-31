# Mock Server and Postman Collection

The specification has no backend. A mock server fills that gap: [Prism](https://github.com/stoplightio/prism) reads [`event-management-api.yaml`](event-management-api.yaml) and answers requests with the examples from the file. The Postman collection sends requests to the mock and checks the responses.

## Run the mock

You need Node.js on a computer.

1. Clone the repository or download `event-management-api.yaml`.
2. Run this command in the folder with the file:

```bash
npx @stoplight/prism-cli mock event-management-api.yaml
```

Prism starts on `http://127.0.0.1:4010`. In this setup Prism serves the endpoints without the `/v1` prefix from the placeholder server URL, so paths start right after the port:

```bash
curl "http://127.0.0.1:4010/events?date=2026-11-14"
```

If Prism returns `NO_PATH_MATCHED_ERROR`, compare the URL with the paths in the specification. Do not add `/v1`.

## Trigger error responses

Prism returns the success example by default. Add the `Prefer` header to receive a documented error instead:

```bash
curl -X PUT "http://127.0.0.1:4010/events/evt-1042/cabin" \
  -H "Content-Type: application/json" \
  -H "Prefer: code=409" \
  -d '{"cabinId": "cabin-vip-02"}'
```

The response is `409 CABIN_ALREADY_BOOKED` with the example from the specification. The same header works for 400, 404, and 422.

## Run the Postman collection

1. In Postman, choose Import and select [`event-management-api.postman_collection.json`](event-management-api.postman_collection.json).
2. Check the collection variable `baseUrl`. The default is `http://127.0.0.1:4010`.
3. Start Prism, then run the collection with the Collection Runner.

The collection holds 18 requests in five folders: Events, Cabins, Services, Staff, and Schedule. Each request name starts with a user story ID from the [traceability matrix](traceability.md). Five requests send a `Prefer` header to show error responses (404, 409, 422). Every request has two test checks: the expected status code and a response check for key contract fields or the documented error code.

## Run the collection with Newman

Newman runs the same collection in the terminal and needs no Postman window. Start Prism in one terminal, then run this command in a second terminal from the folder with the collection:

```bash
npx newman run event-management-api.postman_collection.json --env-var "baseUrl=http://127.0.0.1:4010"
```

Newman lists every request with its result and prints a summary of requests and assertions. The latest run is in [`newman-results.png`](newman-results.png).

## Limits

- Prism checks requests against the schema. A request with a missing required field returns a validation error from Prism, not the documented 400.
- Prism does not store data. A created event does not appear in the list afterwards.
- Responses without an example in the specification come from the schema and contain placeholder values.
