# Ruby Code Snippets
* [Character Counting Hash](#character-counting-hash)
* [Get JSON data via API](#get-json-data-via-api)
* [Get Top K Frequent Elements](#get-top-k-frequent-elements)
* [Convert Arrays to a Hash](#convert-arrays-to-a-hash)
* [Map of Letters to Numbers](#map-of-letters-to-numbers)
* [Ruby Project Structure](#ruby-project-structure)
* [Reading from a File](#reading-from-a-file)
* [Writing to a File](#writing-to-a-file)
* [Require Multiple Files in a Directory](#require-multiple-files-in-a-directory)

### Character Counting Hash

```ruby
words = ['cat', 'skunk', 'ox', 'dog']
words.group_by(&:size)

# Output: {3=>["cat", "dog"], 5=>["skunk"], 2=>["ox"]}
```

# 
### Get JSON data via API
_Note: You need to use `require 'open-uri'` and `require 'json'`_

Without query parameters / API key:
```ruby
request = "https://db.ygoprodeck.com/api/v4/cardinfo.php"
response = open(request).readlines.join		# open(request, &:read) also works
card_catalog = JSON.parse(response)
```

With query parameters / API key:
```ruby
api_key = get_stock_api_key()
keyword = "AAPL"

request = "https://www.alphavantage.co/query?function=SYMBOL&keywords=#{keyword}&apikey=#{api_key}"
response = open(request).readlines.join		# open(request, &:read) also works
search_data = JSON.parse(response)
```

#
### Get Top K Frequent Elements
```ruby
nums = [1,1,1,2,2,3]
k = 2

def top_k_frequent(nums, k)
    counts = Hash.new(0)
    nums.each { |num| counts[num] += 1 }
    counts.keys.sort { |a, b| counts[b] <=> counts[a] } [0...k]
end

top_k_frequent(nums, k)

# Output: [1,2]
```

#
### Convert Arrays to a Hash
```ruby
keys = ["a", "b", "c"]
values = [1, 2, 3]

keys.zip(values).to_h

# Output: {"a"=>1, "b"=>2, "c"=>3}
```

#
### Map of Letters to Numbers
```ruby
letters = 'a'..'z'
numbers = 1..26

letters.zip(numbers).to_h

# Output: {"a"=>1, "b"=>2, "c"=>3, "d"=>4, "e"=>5, "f"=>6, "g"=>7, "h"=>8, "i"=>9, "j"=>10, "k"=>11, "l"=>12, "m"=>13, "n"=>14, "o"=>15, "p"=>16, "q"=>17, "r"=>18, "s"=>19, "t"=>20, "u"=>21,"v"=>22, "w"=>23, "x"=>24, "y"=>25, "z"=>26}
```

#
### Ruby Project Structure
* app/
	* gems/                 # External Libraries
		* Gemfile
		* Gemfile.lock
  	* lib/					# Program Files
	* tests/   				# Test Files
  	* README
	* appname.rb            # Driver 

#
### Reading from a File
Line-by-Line via open():
```ruby
File.open("my/file/path", "r") do |f|
  f.each_line do |line|
    puts line
  end
end
```

Entire File in Memory (if file not large):
```ruby
puts File.read(file_name)
```

Line-by-Line via IO or File:
```ruby
IO.foreach("testfile") { |x| print x }

File.foreach('testfile') { |x| print x }
```

#
### Writing to a File
Generally:
```ruby
File.open(yourfile, 'w') { |file| file.write("your text") }
```

Short Version:
```ruby
File.write('/path/to/file', 'some content')
```

Append to an Existing File:
```ruby
File.write('/path/to/file', 'some content', mode: 'a')
```

#
### Require Multiple Files in a Directory
```ruby
Dir.glob('lib/*.rb') { |f| require_relative f }
```

#
### Find Source Location of a Method
Assuming class is declared in a file `test.rb`:
```ruby
class Dog
	def bark()
		return "woof"
    end
end

d = Dog.new

puts d.method(:bark).source_location

# Output: 
# test.rb
# 2
```