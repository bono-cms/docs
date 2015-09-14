Questions and Answers
=====================

This module let's you manage an answerable FAQ version on your site. That means, that your users will be able to question you directly via form. After they send a form, you'll get a notification and ability to moderate their questions.

# Getting started

To start using this module, you have to create a new page whose controllers leads to `Qa:Qa@indexAction`. Then in your current theme directory you need to create an empty template called `qa.phtml`. In the template, you'd have an array of entity objects, which is called `$pairs`, and has the following methods:

## getId()

Returns unique id.

## getQuestion()

Returns a question.

## getQuestioner()

Returns questioner name.

## getAnswer()

Returns answer to the question.

## getAnswerer()

Returns answerer name.

## getTimestampAsked()

Returns UNIX timestamp of date, when this question has been asked.

## getTimestampAnswered()

Returns UNIX timestamp of date, when this question has been answered.


# Form

To let your users ask questions, you need to have a form on the page. The form itself typically looks like so:

    <form class="form-horizontal" method="POST">
      <fieldset>
    	<legend><?php $this->show('Do you have a question? Just ask us!'); ?></legend>
    	
    	<div class="form-group">
    	  <label for="inputName" class="col-lg-2 control-label"><?php $this->show('Name'); ?></label>
    	  <div class="col-lg-10">
    		<input type="text" class="form-control" name="questioner" id="inputName" placeholder="<?php $this->show('Type your name'); ?>" />
    	  </div>
    	</div>
    	
    	<div class="form-group">
    	  <label for="inputQuestion" class="col-lg-2 control-label"><?php $this->show('Question'); ?></label>
    	  <div class="col-lg-10">
    		<textarea class="form-control" name="question" id="inputQuestion" placeholder="<?php $this->show('Type your question'); ?>"></textarea>
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

