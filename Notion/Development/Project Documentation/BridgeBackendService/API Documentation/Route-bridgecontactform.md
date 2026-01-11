
	Route `/bridgecontactform` is used by home site of bridge solutions to get client information (contact form)

==`Route`==
`/bridgecontactform/register`  
- used to register form client information into the form
- backup form client information into google form (sink)
- send mail to contactbridgesolutions@gmail.com to notify the team about the client information

==`header`==
`request.headers` contains `bridgecontactform-api-key` 
- `bridgecontactform-api-key` (authentication)

==`body`==
`request.body` contains `name` `phone` `email` `description` 
- `name` (optional)
- `phone` ,`email` either one field is mandatory
- `description` (optional)