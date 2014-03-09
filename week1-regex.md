## Regular Expressions 101

#### From Week 1 Rails Study Group


-[Week 1 Transcript](https://github.com/afshinator/OdinRailsStudyGroup/blob/master/week1-transcript.md)

-[Week 1 Google Hangout](https://plus.google.com/u/0/events/cot10jfo8isvp486c9vkut2t33s?authkey=CNvcqOHw37W61AE)

---

#### What is it?

- **A sequence of characters that forms a search pattern,**

- **Mainly for use in pattern matching with strings, or string matching, i.e. "find and replace"-like operations.**

They are : 

- tremendously useful

- everyone hates them, nobody knows how to use them

- everbody who knows how to use them goes on to rule the universe!

- They're *not* going away.  

- Regex syntax is reasonably standard between programming languages

- There is no lack of string searching and pattern matching that you will do in your programming career.  Learn you some regexs and you'll be way ahead of the game.

- often abbreviated as regex, or regexp

+ Ruby provides several ways of initializing regular expressions. The following are all equivalent and create equivalent Regexp objects:


```ruby
/something/
%r{something}		
Regexp.new("something")
Regexp.compile("something")
```


+ Some simple examples

```ruby
	/Perl|Python/			# / delimits pattern, | separates things we're comparing

	/P(erl|ython)/			# You can use parens like in math expressions, same pattern to match as above

	/ab+c/					# Specifying repetition on; 'a' followed by 1 or more 'b's, followed by 'c'
	/ab*c/					# same as above, except 0 or more b's

	/Perl\s+Python/			# \s is whitespace (space, tab, newline), \d digit, \w character, . matches almost any characer
							# so above is 'Perl', whitespace characters, then 'Python'
```

+ The match operator =~ can be used to match a string against a regular expression. 

+ If the pattern is found in the string, =~ returns its starting position, 
	 	otherwise it returns nil.

```ruby
if line =~ /Perl|Python/		# write msg if line contains perl or python
	puts "Scripting language mentioned: #{line}"
end	
```

+ The part of a string matched by a regular expression can be replaced with different text using one of Ruby’s substitution methods.

```ruby
line.sub(/Perl/, 'Ruby') 	# replace first 'Perl' with 'Ruby'
line.gsub(/Python/, 'Ruby') # replace every 'Python' with 'Ruby'
```

#### Tools
- [https://www.debuggex.com/](https://www.debuggex.com/)
- [http://www.rubular.com/](http://www.rubular.com/)