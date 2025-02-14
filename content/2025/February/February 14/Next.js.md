## Build your own Auth

[Lucia](https://lucia-auth.com) no library is a good choice. Lucia is an open source project to provide resources on implementing authentication with JavaScript and TypeScript.

> Why not a library?
> We've found it extremely hard to develop a library that:
> - Supports the many database libraries, ORMs, frameworks, runtimes, and deployment options available in the ecosystem.
> - Provides enough flexibility for the majority of use cases.
> - Does not add significant complexity to projects.
>
> We came to the conclusion that at least for the core of auth - sessions - it's better to teach the code and concepts rather than to try cramming it into a library. The code is very straightforward and shouldn't take more than 10 minutes to write it once you understand it. As an added bonus, it's fully customizable.

I need to delicate more time to read *[The Copenhagen Book](https://thecopenhagenbook.com)*


## Sessions

### Sessions with Prisma

Basic session API

> Users will use a session token linked to a session instead of the ID directly. The session ID will be the SHA-256 hash of the token. SHA-256 is a one-way hash function. This ensures that even if the database contents were leaked, the attacker won't be able retrieve valid tokens.

#### Declare Prisma schema
A session model should be created with a field for a text ID, user ID, and expiration.

```typescript
model User {
  id       Int       @id @default(autoincrement())
  sessions Session[]
}

model Session {
  id        String   @id
  userId    Int
  expiresAt DateTime

  user      User     @relation(references: [id], fields: [userId], onDelete: Cascade)
}
```

The session ID will be SHA-256 hash of the token, which was set to expire in 30 days.

#### Create API

```typescript
import type { User, Session } from "@prisma/client";

export function generateSessionToken(): string {
	// TODO
}

export async function createSession(token: string, userId: number): Promise<Session> {
	// TODO
}

export async function validateSessionToken(token: string): Promise<SessionValidationResult> {
	// TODO
}

export async function invalidateSession(sessionId: string): Promise<void> {
	// TODO
}

export async function invalidateAllSessions(userId: number): Promise<void> {
	// TODO
}

export type SessionValidationResult =
	| { session: Session; user: User }
	| { session: null; user: null };
```
> The session token should be a random string. We recommend generating at least 20 random bytes from a secure source (DO NOT USE Math.random()) and encoding it with base32. You can use any encoding schemes, but base32 is case insensitive unlike base64 and only uses alphanumeric letters while being more compact than hex encoding.

More details click [here](https://lucia-auth.com/sessions/basic-api/prisma)

#### Using your API

When a user signs in, generate a session token with generateSessionToken() and create a session linked to it with createSession(). The token is provided to the user client.
```typescript
import { generateSessionToken, createSession } from "./session.js";

const token = generateSessionToken();
const session = createSession(token, userId);
setSessionTokenCookie(token);
```
Validate a user-provided token with validateSessionToken().

### How to store the token on the client? Cookies

#### CSRF protection
> CSRF protection is a must when using cookies. A very simple way to prevent CSRF attacks is to check the Origin header for non-GET requests. If you rely on this method, it is crucial that your application does not use GET requests for modifying resources.

```typescript
// `HTTPRequest` and `HTTPResponse` are generic interfaces.
// Adjust this code to fit your framework's API.

function handleRequest(request: HTTPRequest, response: HTTPResponse): void {
	if (request.method !== "GET") {
		const origin = request.headers.get("Origin");
		// You can also compare it against the Host or X-Forwarded-Host header.
		if (origin === null || origin !== "https://example.com") {
			response.setStatusCode(403);
			return;
		}
	}

	// ...
}
```

#### Cookies

If the frontend and backend are hosted on the same domain, session cookies should have the following attributes:
- HttpOnly: Cookies are only accessible server-side
- SameSite=Lax: Use Strict for critical websites
- Secure: Cookies can only be sent over HTTPS (Should be omitted when testing on localhost)
- Max-Age or Expires: Must be defined to persist cookies
- Path=/: Cookies can be accessed from all routes

> [!Note]
> Lucia v3 used auth_session as the session cookie name.

```typescript
// `HTTPResponse` is a generic interface.
// Adjust this code to fit your framework's API.

export function setSessionTokenCookie(response: HTTPResponse, token: string, expiresAt: Date): void {
	if (env === Env.PROD) {
		// When deployed over HTTPS
		response.headers.add(
			"Set-Cookie",
			`session=${token}; HttpOnly; SameSite=Lax; Expires=${expiresAt.toUTCString()}; Path=/; Secure;`
		);
	} else {
		// When deployed over HTTP (localhost)
		response.headers.add(
			"Set-Cookie",
			`session=${token}; HttpOnly; SameSite=Lax; Expires=${expiresAt.toUTCString()}; Path=/`
		);
	}
}

export function deleteSessionTokenCookie(response: HTTPResponse): void {
	if (env === Env.PROD) {
		// When deployed over HTTPS
		response.headers.add(
			"Set-Cookie",
			"session=; HttpOnly; SameSite=Lax; Max-Age=0; Path=/; Secure;"
		);
	} else {
		// When deployed over HTTP (localhost)
		response.headers.add("Set-Cookie", "session=; HttpOnly; SameSite=Lax; Max-Age=0; Path=/");
	}
}
```
#### Session validation

Session tokens can be validated using the validateSessionToken() function from the [Basic session API](https://lucia-auth.com/sessions/basic-api/) page.

If the session is invalid, delete the session cookie.
> [!Advise]
> Importantly, Lucia team recommends setting a new session cookie after validation to persist the cookie for an extended time.



## What I've got the resuld like this and filled in this form:

#### Forgot Password Process:

▼ Before verify email:

| | |
|---------|---|
| Account | 1 |
| PasswordRestSession | 1 |
| Session | 0 |
| User | 1 |

▼ After verifying email:

| | |
|---------|---|
| Account | 1 |
| PasswordRestSession | 1 |
| Session | 0 |
| User | 1 |

▼ After resetting password:

Session should be deleted once you have reset the password.
| | |
|---------|---|
| Account | 1 |
| PasswordRestSession | 0(Solved) |
| Session | 0 |
| User | 1 |
