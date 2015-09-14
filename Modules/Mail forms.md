Mail forms
==========

This module lets you create pages that send emails to your inbox, right from your site. As an example, that can be a contact page, or CV from that accepts resumes from users.

# Creating a new page

The page you're going to create might have a different amount of fields. For example, sometimes you need a field called "Phone number", sometimes you don't, so it's not possible to predict a scenario. Therefore, there are no pre-defined fields for your mailing forms. But let's start from the basic terms.

## Message view

Message view is a framework-compliant (i.e without .phtml extension) template name, that is located at `/MailForm/View/Template/messages/`. It defines a message that is going to be sent, once user submits the form. By default, all message views have the `message` template.

In all message templates, you'd have an array called `$input`, which contains all POST data.

## Site template

The site template must be located under current site theme directory. Whatever name you choice when creating it, you'd type it later when creating a new mail form. You should define all fields manually there. For example, when creating a new contact form, you can create a template named `contact-form.phtml`, then set `contact-form` as a value of template option. Finally, the content of a form template itself might look like this:

    <article><?php echo $page->getDescription(); ?></article>
    
    <form class="form-horizontal" method="POST">
      <fieldset>
    	<legend><?php echo $page->getTitle(); ?></legend>
    	
    	<div class="form-group">
    	  <label for="inputName" class="col-lg-2 control-label"><?php $this->show('Name'); ?></label>
    	  <div class="col-lg-10">
    		<input type="text" class="form-control" name="name" id="inputName" placeholder="<?php $this->show('Your name'); ?>" />
    	  </div>
    	</div>
    	
    	<div class="form-group">
    	  <label for="inputEmail" class="col-lg-2 control-label"><?php $this->show('Email'); ?></label>
    	  <div class="col-lg-10">
    		<input type="text" class="form-control" name="email" id="inputEmail" placeholder="<?php $this->show('Type your email if you want us to reply'); ?>" />
    	  </div>
    	</div>
    	
    	<div class="form-group">
    	  <label for="textArea" class="col-lg-2 control-label"><?php $this->show('Message'); ?></label>
    	  <div class="col-lg-10">
    		<textarea class="form-control" rows="3" id="textArea" name="message" placeholder="<?php $this->show('Type your message'); ?>"></textarea>
    	  </div>
    	</div>
    	
    	<div class="form-group">
    		<label for="inputCaptcha" class="col-lg-3 control-label"></label>
    		<div class="col-lg-9">
    			<a href="#" title="Click to refresh" data-captcha="button-refresh"><img data-captcha="image" src="/captcha/render/" /></a>
    		</div>
    	</div>
    	
    	<div class="form-group">
    		<label for="inputCaptcha" class="col-lg-2 control-label"></label>
    		<div class="col-lg-10">
    			<input type="text" class="form-control" name="captcha" placeholder="<?php $this->show('Enter what you see on image'); ?>" />
    		</div>
    	</div>
    	
    	<div class="form-group">
    	  <div class="col-lg-10 col-lg-offset-2">
    		<button type="reset" class="btn btn-default"><i class="glyphicon glyphicon-remove"></i> <?php $this->show('Cancel'); ?></button>
    		<button type="submit" class="btn btn-primary" data-button="submit"><i class="glyphicon glyphicon-envelope"></i> <?php $this->show('Submit'); ?></button>
    	  </div>
    	</div>
    	
      </fieldset>
    </form>

## Validation

So far, we created a new page that already works and is able to sent visitor messages right to your inbox. But one thing we missed is a form validation, which is extremely important. All validation rules for fields are defined in `/MailForm/Controller/Form::getValidationRules` method. That method simply returns an array of rules for particular form ids. For example, for above aforementioned template, we can define rules like so:

	use Krystal\Validate\Pattern; // <- Add this import at the top, right after namespace declarion

	//......


	private function getValidationRules(array $input)
	{
		return array(
			'1' => array(
				'input' => array(
					'source' => $input,
					'definition' => array(
						'name' => new Pattern\Name(),
						'email' => new Pattern\Email(),
						'message' => new Pattern\Message(),
						'captcha' => new Pattern\Captcha($this->captcha)
					)
				)
			)
		);
	}

	// ....

'1' is an id of the form. To get an id itself, you can simply take a look at module's table grid, at `#` column. As you can see `name`, `email`, `message` and `captcha` are expected form field names. Their values are validation patterns that represent rules. Nothing fancy with the rules and patterns - they are just declarations of validation component in Krystal Framework. To learn a bit, more, please refer to Krystal's validation component.
