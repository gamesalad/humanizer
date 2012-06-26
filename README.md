# Humanizer

Humanizer is a very simple CAPTCHA method. It has a localized YAML file with questions and answers which is used to validate that the user is an actual human. Any model that includes ActiveModel::Validations should work. Our aim is to be database and mapper agnostic, so if it doesn't work for you, open an issue. Humanizer only works with Rails 3.

## Installation

1. `gem install humanizer`
2. `rails g humanizer`

## Advanced Installation

* Install all locales: `rails g humanizer --all-locales`
* Show available locales: `rails g humanizer --show-locales`
* Install selected locales: `rails g humanizer en fi de`

## Usage

1. In your model, include Humanizer and add the #require_human_on method, example:

        class User < ActiveRecord::Base
          include Humanizer
          require_human_on :create
        end

2. Ask the question in the form, example:

        <%= f.label :humanizer_answer, @model.humanizer_question %>
        <%= f.text_field :humanizer_answer %>
        <%= f.hidden_field :humanizer_question_id %>

3. If you are using attr_accessible, remember to whitelist `:humanizer_answer` and `:humanizer_question_id`.

## Configuration

Default translations can be found in config/locales/

You might want to add/change question and answer pairs. This can be easily done by adding/modifying entries in locales file.

## Multiple Lists and Reloading questions

You can have more then one list of questions, perhaps if a user gets a question wrong, you may want to choose one from another list. Add the different lists as such:

    questions:
      - question: Two plus two?
        answers: ["4", "four"]
      - question: Jack and Jill went up the...
        answer: hill
    games:
      - question: What color is pacman?
        answer: yellow
      - question: What is the name of mario's princess?
        answers: ["peach","princess peach"]

questions is the default list, and then you can have other named lists.

Change the list as so:

      @user.change_list("games")

## Skipping validation

You might want to skip the humanizer validations on your tests or rails console.

You can just have a simple attribute on your model and use it to bypass the validation. Here's an example:

    attr_accessor :bypass_humanizer
    require_human_on :create, :unless => :bypass_humanizer

Now when bypass_humanizer is true, validation will be skipped.

In case you want to give your users the option to change the question, there's a #change_humanizer_question method to help you.

To make sure the current question doesn't get asked again, you can pass the current question id to the method. For example:

    @user.change_humanizer_question(params[:user][:humanizer_question_id])
## Different Question Lists
          
## Live sites

* [ArcticStartup.com](http://arcticstartup.com/) - sign up form
* [Paspartout](http://paspartout.com/) - sign up form

## License

Humanizer is licensed under the MIT License, for more details see the LICENSE file.

## Question/Answer Translations

* English, Finnish and Portuguese translations by [Kisko Labs](http://kiskolabs.com/)
* German by [Sven Schwyn](http://github.com/svoop)
* Dutch by [Joren De Groof](http://github.com/joren)
* Brazilian Portuguese by [Britto](http://github.com/britto)
* Russian by [Shark](http://github.com/Serheo)
* Spanish by [Juanjo Bazán](https://github.com/xuanxu)
* Polish by [Maciek O](https://github.com/ohaleck)

## Contributors

* [Florian Bertholin](https://github.com/Arkan)
* [seogrady](https://github.com/seogrady)
* [yairgo](https://github.com/yairgo)
* [rubygeek](https://github.com/rubygeek)

## CI Build Status

[![Build Status](http://travis-ci.org/kiskolabs/humanizer.png)](http://travis-ci.org/kiskolabs/humanizer)
