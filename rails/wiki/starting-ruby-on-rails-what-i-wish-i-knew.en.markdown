## Starting Ruby on Rails: What I Wish I Knew
> from betterexplained.com , update by francis

[Ruby on Rails](http://rubyonrails.com/) is an **elegant**, **compact** and **fun** way to build web applications. Unfortunately, many gotchas await the new programmer. Now that I have a few [rails projects}(http://instacalc.com/) under my belt, here's my shot at sparing you the suffering I experienced when first getting started.

### Tools: Just Get Them

Here's the tools you'll need. Don't read endless reviews trying to decide on the best one; start somewhere and get going.

* [Agile Web Development with Rails.](http://www.pragmaticprogrammer.com/titles/rails/) Yes, it's a book. And a cohesive book is worth 100 hobbled-together online tutorials.
* [InstantRails](http://rubyforge.org/projects/instantrails/): A `.zip` file containing Ruby, Apache, MySQL and PHP (for PhpMyAdmin), packaged and ready to go.
* [Aptana/RadRails](http://www.aptana.com/) (like Eclipse) or Ruby In Steel (like Visual Studio) for editing code.
* [Subversion](http://subversion.tigris.org/), [TortoiseSVN](http://tortoisesvn.net/downloads) and/or [Github](https://github.com/) for source control.
* Browse popular ruby on rails links on [del.icio.us]http://del.icio.us/popular/rubyonrails), [Rails documentation](http://railsbrain.com/) and [Ruby syntax & examples](http://docs.huihoo.com/ruby/ruby-man-1.4/syntax.html).

But What Does It All Mean?

"Ruby on Rails" is catchy but confusing. Is Rails some type of magical drug that Ruby is on? (Depending on who you ask, yes.)

**Ruby** is a programming language, similar to +Python+ and +Perl+. It is dynamically typed (no need for "int i"), 解释执行, and can be modified at runtime (such as adding new methods to classes). It has 数十种 of shortcuts that make it very clean; methods are rarely over 10 lines. It has good RegEx support and works well for shell scripting.

**Rails** is a [gem](http://rubygems.org/read/book/1), or a Ruby library. Some gems let you use the Win32 API. Others handle networking. Rails helps make web applications, providing classes for saving to the database, handling URLs and displaying html (along with a webserver, maintenance tasks, a debugging console and much more).

**IRB** is the interactive Ruby console (type "irb" to use). Rails has a special IRB console to access your web app as it is running (excellent for live debugging).

**Rake** is Ruby's version of Make. Define and run maintenance 维护 tasks like setting up databases, reloading data, backing up, or even deploying an app to your website.

**Erb** is embedded Ruby, which is like PHP. It lets you mix Ruby with HTML (for example): Ruby, which is like PHP. It lets you mix Ruby with HTML (for example):

  ```erb
  <div>Hello there, <%= get_user_name() %></div>
  ```
**YAML** (or **YML**) means "YAML Ain't a Markup Language" — it's a simple way to [specify data](http://en.wikipedia.org/wiki/YAML):

  ```yaml
  {name: John Smith, age: 33}
  ```

It's like JSON, much leaner than XML, and used by Rails for setting configuration options (like setting the database name and password).

Phew! Once Ruby is installed and in your path, you can add the rails gem using:

  ```shell
  gem install rails
  ```

In general, use gem install "gem_name", which searches online sources for that library. Although Rails is "just another gem", it is the killer library that brought Ruby into the limelight.

### Understanding Ruby-Isms

It's daunting to learn a new library and a new language at the same time. Here are some of the biggest Ruby gotchas 陷阱 for those with a C/C++/Java background.

Ruby removes unnecessary cruft: (){};

  * Parenthesis on method calls are optional; use print "hi".
  * Semicolons aren't needed after each line (crazy, I know).
  * Use "if then else end" rather than braces.
  * Parens aren't needed around the conditions in if-then statements.
  * Methods automatically return the last line (call return explicitly if needed)

Ruby scraps the annoying, ubiquitous punctuation that distracts from the program logic. Why put parens ((around),(everything))? Again, if you want parens, put ‘em in there. But you'll take off the training wheels soon enough.

The line noise (er, "punctuation") we use in C and Java is for the compiler's benefit, not ours. Be warned: after weeks with Ruby, other languages become a bit painful to read.

<pre><ruby>
def greet(name)              # simple method
   "Hello, " + name          # returned automatically
end

greet "world"                # ==> "Hello, world"
</ruby></pre>

**Those Funny Ruby Variables**

  * `x = 3` is a local variable for a method or block (gone when the method is done)
  * `@x = 3` is a instance variable owned by each object (it sticks around)
  * `@@x = 3` is a class variable shared by all objects (it sticks around, too).
  * `:hello` is a symbol, like a constant string. Useful for indexing hashes. Speaking of which…
  * `dictionary = { :cat => "Goes meow", :dog => "Barks loud."}` is a hash of key/value pairs. Access elements with `dictionary[:cat]`.
    > when key is a symbol, after ruby 1.9 `dictionary = { "cat" => "Goes meow", dog: "Barks loud."}`

**Those Funny Ruby Assignments**

Ruby has the `||` operator which is a bit funky. When put in a chain

  ```ruby
  x = a || b || c || "default"
  ```

it means "test each value and return the first that's not false." **So if a is false, it tries b. If b is false, it tries c. Otherwise, it returns the string "default".**

If you write `x = x || "default"` it means "set x to itself (if it has a value), otherwise use the default." An easier way to write this is

  ```ruby
  x ||= "default"
  ```

which means the same: set x to the default value unless it has some other value. You'll see this a lot in Ruby programs.

**Those Funny Ruby Blocks**

Ruby has "blocks", which are like anonymous functions passed to a loop or another function. These blocks can specify a parameter using `|param|` and then take actions, call functions of their own, and so on. Blocks are useful when applying some function to each element of an array. It helps to think of them as a type of anonymous function that can, but doesn't have to, take a parameter.

<pre><ruby>
3.times do |i|
   print i*i
end
</ruby></pre>

In this example, the numbers `0`,`1` and `2` are passed to a block (do… end) that takes a single parameter (i) and prints i squared. The output would be `0`, followed by `1` followed by `4` (and looks like "014″ since we didn't include spaces). Blocks are common in Ruby but take some getting used to, so be forewarned.

These are the Ruby lessons that were tricky when starting out. Try [Why's Poignant Guide To Ruby](http://mislav.uniqpath.com/poignant-guide/) for more info ("Why" is the name of the author… it confused me too).

### Understanding Rails-isms
Rails has its own peculiarities. "Trust us, it's good for you." say the programmers. It's true – the features/quirks make Rails stand out, but they're confusing until they click. Remember:

  * **Class and table names are important**. Rails has certain naming conventions; it expects objects from the class Person to be saved to a database table named people. Yes, Rails has **a pluralization engine** to figure out what object maps to what table (I kid you not). This magic is great, but scary at first when you're not sure how classes and tables are getting linked together.
  * Many methods take an "options" hash as a parameter, rather than having dozens of individual parameters. When you see

  ```ruby
  link_to "View Post", :action => 'show', :controller => 'article', :id => @article
  ```

The call is really doing this:

  ```ruby
  link_to("View Post", {:action => 'show', :controller => 'article', :id => @article})
  ```

There are only two parameters: the name ("View Post") and a hash with 3 key/value pairs. Ruby lets us remove the extra parens and braces, leaving the stripped-down function call above.

### Understanding The Model-View-Controller Pattern

Rails is built around the [model-view-controller](http://betterexplained.com/articles/intermediate-rails-understanding-models-views-and-controllers/) pattern. It's a **simple concept**: **separate the data, logic, and display layers of your program**. This lets you split functionality cleanly, just like having separateHTML, CSS and Javascript files prevents your code from mushing together. Here's the MVC breakdown:

  * **Models** are classes that talk to the databse. You find, create and save models, so you don't (usually) have to write SQL. Rails has a class to handle the magic of saving to a database when a model is updated.
  * **Controllers** take user input (like a URL) and decide what to do (show a page, order an item, post a comment). They may initially have business logic, like finding the right models or changing data. As your rails ninjitsu improves, constantly refactor and move business logic into the model ([fat model, skinny controller](http://weblog.jamisbuck.org/2006/10/18/skinny-controller-fat-model)). Ideally, controllers just take inputs, call model methods, and pass outputs to the view (including error messages).
  * **Views** display the output, usually HTML. They use ERB and this part of Rails is like PHP - you useHTML templates with some Ruby variables thrown in. Rails also makes it easy to create views asXML (for web services/RSS feeds) or JSON (for AJAX calls).

The MVC pattern is key to building a readable, maintainable and easily-updateable web app.

### Understanding Rails' Directory Structure

When you create your first rails app, the directories are laid out for you. The structure is well-organized: Models are in `app/models`, controllers in `app/controllers`, and views in `app/my_local_views` (just kidding).

The naming conventions are important – it lets rails applications "find their parts" easily, without additional configuration. Also, it's very easy for another programmer to understand and learn from any rails app. I can take a look at Typo, the rails blogging software, and have a good idea of how it works in minutes. Consistency creates comprehension.

### Understanding Rails' Scaffolding

**Scaffolding** gives you default controller actions (URLs to visit) and a view (forms to fill out) to interact with your data — you don't need to build an interface yourself. You do need to define the Model and create a database table.

Think of scaffolds as the "default" interface you can use to interact with your app – you'll slowly override parts of the default as your app is built. You specify scaffolds in the controller with a single line:

and it adds default actions and views for showing, editing, and creating your "Person" object. Rails forms take some getting used to, so scaffolding helps a lot in the initial stages.

### More Tips and Tricks

I originally planned on a list of tips & tricks I found helpful when learning rails. It quickly struck me that Ruby on Rails actually requires a lot of background knowledge, and despite (or because of) its "magic", it can still be confusing. I'll get into my favorite tricks in an upcoming article.

As you dive further into web development, these guides may be helpful:

  * [How To Debug Web Applications With Firefox](http://betterexplained.com/articles/how-to-debug-web-applications-with-firefox/)
  * [How To Optimize Your Site With HTTP Caching](http://betterexplained.com/articles/how-to-optimize-your-site-with-http-caching/)
  * [How To Optimize Your Site With GZIP Compression](http://betterexplained.com/articles/how-to-optimize-your-site-with-gzip-compression/)
  * [Speed Up Your Javascript Load Time](http://betterexplained.com/articles/speed-up-your-javascript-load-time/)
  * [The Quick Guide to GUIDs](http://betterexplained.com/articles/the-quick-guide-to-guids/)

Source : [http://betterexplained.com/articles/starting-ruby-on-rails-what-i-wish-i-knew/](http://betterexplained.com/articles/starting-ruby-on-rails-what-i-wish-i-knew/)
