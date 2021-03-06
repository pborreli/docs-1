# Mail

- [Configuration](#configuration)
- [Basic Usage](#basic-usage)
- [Embedding Inline Attachments](#embedding-inline-attachments)

<a name="configuration"></a>
## Configuration

Laravel provides a clean, simple API over the popular [SwiftMailer](http://swiftmailer.org) library. The mail configuration file is `app/config/mail.php`, and contains options allowing you to change your SMTP host, port, and credentials, as well as set a global `from` address for all messages delivered by the library. You may use any SMTP server you wish.

<a name="basic-usage"></a>
## Basic Usage

The `Mail::send` method may be used to send an e-mail message:

	Mail::send('emails.welcome', $data, function($m)
	{
		$m->to('foo@example.com', 'John Smith')->subject('Welcome!');
	});

The first argument passed to the `send` method is the name of the view that should be used as the e-mail body. The second is the `$data` that should be passed to the view, and the third is a Closure allowing you to specify various options on the e-mail message.

> **Note:** A `$message` variable is always passed to e-mail views, and allows the inline embedding of attachments. So, it is best to avoid passing a `message` variable in your view payload.

You may also specify a plain text view to use in addition to an HTML view:

	Mail::send(['html.view', 'text.view'], $data, $callback);

You may specify other options on the e-mail message such as any carbon copies or attachments as well:

	Mail::send('emails.welcome', $data, function($m)
	{
		$m->from('us@example.com', 'Laravel');

		$m->to('foo@example.com')->cc('bar@example.com');

		$m->attach($pathToFile);
	});

When attaching files to a message, you may also specify a MIME type and / or a display name:

	$m->attach($pathToFile, ['as' => $display, 'mime' => $mime]);

> **Note:** The message instance passed to a `Mail::send` Closure extends the SwiftMailer message class, allowing you to call any method on that class to build your e-mail messages.

<a name="embedding-inline-attachments"></a>
## Embedding Inline Attachments

Embedding inline images into your e-mails is typically cumbersome; however, Laravel provides a convenient way to attach images to your e-mails and retrieving the appropriate CID.

**Embedding An Image In An E-Mail View**

	<body>
		Here is an image:

		<img src="<?php echo $message->embed($pathToFile); ?>">
	</body>

**Embedding Raw Data In An E-Mail View**

	<body>
		Here is an image from raw data:

		<img src="<?php echo $message->embedData($data, $name); ?>">
	</body>

Note that the `$message` variable is always passed to e-mail views by the `Mail` class.
