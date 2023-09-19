# Lecture 29: Intro to Ruby

### Intro to Ruby
* created in 1995 by 'Matz'
* he wanted to create a language that was pleasant to read & write, that was as close to English as possible 
  * i.e., very few symbols ```; {} ()```
* in 2005, Rails was created
* in 2015, Rails fell off in 2015 with the introduction of React & ES6
* currently sitting at the #10 most popular language in the world
* purely synchronous 

### So.. why learn Ruby & Rails?
* Ruby is a true object-oriented programming language (OOP)
* Rails is a Model View Controller (MVC) Framework

### Ruby Basics
```rb
# single line comment

=begin
this is a
multi-line
comment
=end

print "hello world, with no new line"
puts "hello world, with a new line" # === console.log()

p "hello" # will print with the type, i.e. will print "hello" instead of hello

gets "What is your favourite colour?" # create a prompt, wait for the user to type something in and press enter

# === let
name = 'Alice'
name = []
name = true

# constants, except Ruby WILL let you reassign (although it will give you a warning)
Name = 'Bob'
Name = 'Carol'

# typically you will see the whole thing capitalized, even though you only need the first character capitalized
NAME = 'Bob'
NAME = 'Carol'

# snake_case for variable names
first_name = 'Alice'
last_name = 'Wonderland'

# concatonation
puts first name + ' ' + last_name

# interpolation NEEDS double quotes " "
puts "#{first_name} #{last_name}"

# equality - really no need to distinguish between == & ===, as there is no type coercion in Ruby
# this results in always using == 
puts 2 == '2' # false (would be true in JS)
puts 2 === '2' # false

puts 2 == '2'.to_s # true
```

### Ruby Conditionals
```rb
name = 'Dean'

# can also use if (name == 'Dean')
if name == 'Dean' # {
  puts "good name!"
elsif name == 'Alice'
  puts "even better name!"
else
  puts "choose a better name"
end # }

# Option to use inverted if with 'unless'
if name != 'Carol'
end
# ==
unless name == 'Carol'
end

# ternary's are identical to JS
puts name == 'Carol' ? 'good name' : 'bad name'

# single-line if-conditions
num = 10
puts "the number is 10" if num == 10

# falsey values in Ruby => nil, false
# truthy values in Ruby => everything else
sunny = false
puts "take an umbrella" unless sunny
```

### Ruby Loops
```rb
# do... end

# loops infinitely
loop do
  puts "hello"
end

# loops once
loop do
  puts "hello"
  break
end

# loops 10 times
loop do
  i += 1 # i++ and i-- do not exist in Ruby
  puts "hello #{i}"

  # single-line == break if (i > 10)
  if (i > 10)
    break
  end
end

# ==

while (i < 10) do
  i += 1
  puts "hello #{i}"
end

# until is an inverted while
game_over = false

while (!game_over) do
end
# ==
until (game_over) do
end

# loop over an array
# Ruby's for..in == JS's for..of
names = ['Alice', 'Bob', 'Carol']

for name in names do
  puts "hello there #{name}"
end

# JS: names.forEach((name) => {})
names.each do |name|
  puts "hello #{name}"
end
```

### Interactive Ruby Shell (irb)
* can play around with this in your terminal!
* to figure out everything you can do with a string, for example, try inputting ```'hello'.methods``` into your irb REPL

### Ruby Methods
* methods == functions
  * ! in a method == will mutate the string (instead of returning a new one)
  * ? in a method == will return a boolean value

```rb
# def in Ruby == function in JS
def say_hello (name)
  puts "hello there #{name}"
  return 42 # do not need to include 'return' - Ruby automatically returns last line of code
end

# functions MUST pass-in the exact number of arguments it expects
# both of these will return an error
say_hello()
say_hello('Bob', 43)
```

### Why is Ruby so much slower than languages like JS & Python?
* In Ruby, *everything* is an object!
  * Primitives in JS, like strings and integers, are objects in Ruby
* How does Ruby get around this? => **SYMBOLS**
  * Symbols are lightweight strings that are as close as you can get to a primitive in Ruby

### Ruby Hashes
* hash == collection of key/value pairs
  * we call these objects in JS, but objects in Ruby are something else!

```rb
# this is old syntax that uses strings, which we know in Ruby are objects
user = {
  "username" => "jstamos",
  "password" => "1234"
}

# this will NOT work - dot (.) syntax will always call a function / method
puts user.username

# this will work! Always use square brackets to access key-value pairs
puts user["username"]

# new syntax takes advantages of symbols!
user = {
  :username => "jstamos",
  :password => "1234"
}

# the NEWEST syntax looks like JS! Note that these are still using symbols.
user = {
  username: "jstamos",
  password: "1234"
}

puts user[:username]
```

### Blocks and Procs
```rb
animals = ['cat', 'dog', 'horse']

# block is denoted with do..end
animals.each do |animal|
  puts "the animal is #{animal}"
end

# Proc(edure) is a block stored in memory
my_block = Proc.new do |animal|
  puts "the animal is #{animal}"
end

animals.each my_block # this won't work! It's still in proc form
animals.each &my_block # & converts it back into a block, which is executable code

# functions / methods accept INVISIBLE blocks, i.e. we can't see if one is passed in as an argument
# only way to tell if a method accepts a block is if you see 'yield'
def accepting_block
  yield # == callback()
end

accepting_block &my_block # passing in invisible block
```

### Ruby Classes
* in Ruby, all properties of classes are PRIVATE by default!
  * contrast this with JS, where all properties are PUBLIC (and there's no way of making them private, other than including the '_' in the name but it's still public)
* Properties can be in four privacy states in Ruby:
  1. Private
  2. Readable
  3. Writable 
  4. Readable and Writable

```rb
# Create a new class
class Car
  # JS => constructor()
  def initialize(make, model, year)
    # JS => this.name vs. Ruby => @name
    @make = make
    @model = model
    @year = year
  end 

  def make # getter
    @make
  end

  def make = (new_make) # setter
    @make = new_make
  end

  # Easier syntax for getter & setter
  attr_reader :model # getter 
  attr_writer :model # setter 

  # Getter & setter in one-line
  attr_accessor :year # getter & setter

  # Getter & setter for every property in one-line
  attr_accessor :make, :model, :year
end

# Instantiate an instance of the class
# JS => car = new Car vs. Ruby => car = Car.new
car = Car.new('Toyota', 'Tercel', 1986)

# Will show the class type & address in memory
puts car

# Will show the class type, address in memory, as well as the properties
p car
```