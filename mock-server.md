Mock Server and Postman Collection

The specification has no backend. A mock server fills that gap: Prism reads event-management-api.yaml and answers requests with the examples from the file. The Postman collection sends requests to the mock and checks the responses.

Run the mock

You need Node.js on a computer.

Clone the repository or download event-management-api.yaml.
Run this command in the folder with the file:
bash
npx @stoplight/prism-cli mock event-management-api.yaml

Prism starts on http://127.0.0.1:4010. In this setup Prism serves the endpoints without the /v1 prefix from the placeholder server URL, so paths start right after the port:

bash
curl "http://127.0.0.1:4010/events?date=2026-11-14"

If Prism returns NO_PATH_MATCHED_ERROR, compare the URL with the paths in the specification. Do not add /v1.

Trigger error responses

Prism returns the success example by default. Add the Prefer header to receive a documented error instead:

bash
curl -X PUT "http://127.0.0.1:4010/events/evt-1042/cabin" \
  -H "Content-Type: application/json" \
  -H "Prefer: code=409" \
  -d '{"cabinId": "cabin-vip-02"}'

The response is 409 CABIN_ALREADY_BOOKED with the example from the specification. The same header works for 400, 404, and 422.

Run the Postman collection
In Postman, choose Import and select postman/event-management-api.postman_collection.json.
Check the collection variable baseUrl. The default is http://127.0.0.1:4010.
Start Prism, then run the collection with the Collection Runner.

The collection holds 18 requests in five folders: Events, Cabins, Services, Staff, and Schedule. Each request name starts with a user story ID from the traceability matrix. Five requests send a Prefer header to show error responses (404, 409, 422). Every request has two tests: the expected status code and a JSON body.

Limits
Prism checks requests against the schema. A request with a missing required field returns a validation error from Prism, not the documented 400.
Prism does not store data. A created event does not appear in the list afterwards.
Responses without an example in the specification come from the schema and contain placeholder values.
