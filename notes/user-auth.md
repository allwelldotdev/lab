How is user-auth implemented? This note seeks to answer that question clearly.

## Technologies
Some technologies to know:
- *OAuth2:* A way for apps to delegate authorization to a third-party.
	- Example: For users to sign into your app, they can use their Google or other social accounts.
- *JWT:* *JSON Web Tokens* can be used in conjunction with OAuth2 to allow your application to verify a users identity and authentication status.

## Step-by-step Process
- User clicks **login** button
- Your app sends user to OAuth Provider login page (Google account login page)
- Once user authenticates they are redirected back to your application with an **authorization code**.
- Your app now needs to exchange this **authorization code** for *refresh* and *access* tokens. (The API in your backend will facilitate this by proxying a request to the authorization provider)
- The *access* token is what your app includes in backend requests as proof of the user's identity.
	- Your backend can validate this token without having the contact the authorization provider.
	- This is what JWTs does.

> In the step-by-step process, you can see how the technologies **OAuth2** and **JWT** are used and work together.

## OAuth2.0
OAuth2 provides two out of the three config IDs you'd need:
- a `client_id` (provided by OAuth2 provider)
- `auth_url` (provided by OAuth2 provider)
- `token_url`

