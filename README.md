# Postman Automation Projects

Postman collections for two small API flows:

- `library.postman_collection.json`: Add/Get/Delete book workflow with data-driven fields and scripted clean-up.
- `oauth.postman_collection.json`: Three-step OAuth demo (get auth code, swap for access token, call protected resource).

## Prerequisites
- Postman Desktop (latest).

## Import and environment setup
1. Import both JSON collections into Postman (File → Import → Upload Files).
2. Create a Postman **Environment** (e.g., `LibraryEnv`) with variables:
	 - `base_url` – API host, e.g., `https://rahulshettyacademy.com` (or your target host).
	 - `book_id` – leave blank; tests will set it after `AddBook`.
3. Add a **Global** variable `companyCode` (a short prefix used to build the ISBN, e.g., `ACME`).
4. Save the environment and select it before running requests.

## Running the Library collection
The collection wires together `AddBook → GetBook → DeleteBook` with scripts that share variables.

Recommended runner setup:
- Data file (CSV/JSON) with columns `BookName,Author` (used to set name/author per iteration). You can also use `test-data/booksData.csv`. Example CSV:

```
BookName,Author
Clean Architecture,Robert Martin
Pragmatic Programmer,Andrew Hunt
```

- Select the environment you created, then run the collection. The `AddBook` test stores `book_id` in the environment, `GetBook` asserts the author matches the iteration data, and `DeleteBook` cleans up. Duplicate book attempts are handled in the tests by re-routing to delete when needed.

(Export your environment as JSON first, and point `-d` to your data file.)

## Running the OAuth collection
1. Update the request query values for `client_id`, `client_secret`, and `redirect_uri` with your own credentials (do **not** commit real secrets).
2. Hit **Get Code** to retrieve an authorization code (you may need to sign in and approve the consent screen).
3. Copy the returned `code` into the **Get Access Token** request (or set it as a Postman variable) and send to obtain an `access_token`.
4. Paste the `access_token` into the **Actual Request** query param and send to call the protected resource.

## Notes
- The collections assume HTTPS endpoints; if you use HTTP locally, update `base_url` accordingly.
- Rotate any shared credentials or tokens before sharing the repo publicly.
