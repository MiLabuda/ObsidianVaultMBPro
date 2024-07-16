1. Factory method
	- If there are multiple ways to create an object of a certain class its good choice
	- Factory method are little better than plain constructor because you can give its descriptive name
	- You make the constructor private and then use the fabric method.
	- Basically this way is just wrapper for constructor
2. Abstract factory
	- Go to solution when creating an object is non-trivial 
	- abstract factories are special kind of service which can have dependencies which they can provide to the created object(in example MailTemplateGenerator while creating Mails)
