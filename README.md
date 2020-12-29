# Regex - re.findall(), re.search(), and re.match() functions

Regular Expressions, also known as Regex, comes in handy in a multitude of text processing scenarios. You can search for patterns of numbers, letters, punctuation, and even whitespace. Regex is fast and helps avoid unnecessary loops in your program to match and extract desired information. Until recently I felt that Regex was very complicated, the syntax looks frustrating and thought that I would not be able to learn about it. As with many others, we share this same feeling.

After reading many resources online, I’ve decided to use this post to show how you can use the “re” module in Python to solve certain problems through the findall() function and briefly introduce match(), and search() functions; all are similar, but have different uses.

**re.findall(): Finding all matches in a string/list**
1. Extract all occurrences of specific words
Using the following text paragraph, which describes how Rey, an African penguin and Rosa, the oldest sea otter at the Monterey Bay Aquarium, require eye drops.
```
aquarium='Because of problems with her eyesight, rey the African penguin had issues with swimming. That’s unusual for a penguin, and presented a big challenge for our aviculture team to help Rey overcome her hesitancy. Slowly and steadily, we trained her to be comfortable feeding in the water like the rest of the penguin colony. The aviculturists also trained Rey to accept daily eye drops from them as part of her special health care. Rey already had good relationships with some staff, and was comfortable with them handling her. Senior Aviculturist Kim Fukuda says the team built on those bonds to get Rey used to receiving the eye drops. "She knows the routine," Kim says. "I usually give her the eye drops in one area of the exhibit after all the penguins get their vitamins. When that happens, she runs over there and waits for me." Rosa, our oldest sea otter, has very limited eyesight, among other health issues. The sea otter team had already trained Rosa so they could examine her eyes, and built on that trust to include administering the eye drops she needs.'
```
Now, you want to extract all occurrences of Rey from the text, for which you would do something like this:
```
rey_occurences = "Rey"
re.findall(rey_occurences, aquarium)
# Output
['Rey', 'Rey', 'Rey', 'Rey']
The findall() function takes two parameters, the first is the pattern being searched, in our case, rey_occurrences and the second parameter is text we are searching through, in our case, aquarium. As you can see, this function returns all the non-overlapping matches of the pattern which is in the rey_occurrences variables, from the second parameter aquarium.
But wait, there is one more rey that was not accounted for. This happened because by default, regular expressions are case sensitive so our findall() function did not return “rey” because it is lowercase and not uppercase as it was defined in the rey_occurrences variables. We can edit our previous code so that it includes lowercase values of the pattern being searched by including a third parameter, flags, which can be used for a variety of reasons such as allowing patterns to match specific lines instead of the whole text, match patterns spanning over multiple lines, and do case-insensitive matching. For our purposes, we will be using the re.IGNORECASE flag to ignore the case while performing the search.
rey_occurences = "Rey"
re.findall(rey_occurences,aquarium,flags=re.IGNORECASE)
# Output
['rey', 'Rey', 'Rey', 'Rey', 'Rey']
```
We can also search multiple patterns and extract all occurrences of those patterns. With our current text, let’s also search for occurrences of “Rosa” by simply using the | operator to create the pattern.
```
sea_animals="Rey|Rosa"
re.findall(sea_animals,aquarium,flags=re.IGNORECASE)
# Output
['rey', 'Rey', 'Rey', 'Rey', 'Rey', 'Rosa', 'Rosa']
```

2. Extracting words that only contain alphabets
Let’s say you have a text document that contains numbers and words such as:
```
gifts = "\
Basketball    2    25.63\
Tshirt     4   53.92\
Sneakers    1    30.58\
Mask    10   80.54\
GiftCard    2    50.00"
```
And let’s say that you want to extract only the words; we can do this by using Regex’s special sequences and sets to specify the pattern we are looking for. This helpful site provides a nice Regex cheatsheet.
```
words = '[a-z]+'
re.findall(words,gifts,flags=re.IGNORECASE)
# Output
['Basketball', 'Tshirt', 'Sneakers', 'Mask', 'GiftCard']
```

3. Extracting all occurrences of numbers
We’ve only shown extracting words from text, can we also extract numbers? Of course, using the regular expressions from another helpful cheat sheet, we are able to extract numbers from the given text:
```
text = "Sixty-six undergraduate students from an urban college in New York City participated in this study. Students participated in this research study as part of a requirement for a class. Nineteen participants were excluded from the study for meeting one or more exclusion criteria including not completing the sentence unscrambling task, not providing ratings for each of the traits, or failing either one of the two attention checks, leaving the total number of eligible participants at forty-seven. Participants included 36 women aged 18 to 52 years old (M = 20.52, SD = 6.97), 10 men aged 18 to 28 years old (M = 23.5, SD = 12.36), and one individual who did not disclose their sex."
numbers="\d+"
re.findall(numbers,text)
# Output
['36', '18','52','20', '52','6','97','10','18','28','23','5','12','36']
By setting our pattern to \d, this signifies to one digit, while the + operator will include repetitions of digits. As you can see from our text, we also have decimals, but from our output they were separated by the “.” We can correct this by using the following regular expression:
all_numbers="\d+\.*?\d+"
re.findall(all_numbers,text)
# Output
['36', '18', '52', '20.52', '6.97', '10', '18', '28', '23.5', '12.36']
```
4. Extracting words that are followed by a specific pattern
With text data, there may be times where you need to extract words that are followed by a special character such as @ for usernames or quotes within the given text.
From our aquarium text, there are two quotes within the text, let’s extract these from the text.
```
quotes='"(.*?)"'
re.findall(quotes,aquarium)
# Output
['She knows the routine,',
 'I usually give her the eye drops in one area of the exhibit after all the penguins get their vitamins. When that happens, she runs over there and waits for me.']
 ```


**re.match(): Returning first occurrence in text**
While re.findall() returns matches of a substring found in a text, re.match() searches only from the beginning of a string and returns match object if found. However, if a match is found somewhere in the middle of the string, it returns none.
The expression “w+” and “\W” will match the words starting with letter ‘r’ and thereafter, anything which is not started with ‘r’ is not identified. To check match for each element in the list or string, we run aforloop in this Python re.match() example:
```
list = ["red rose", "ruby red", "pink peony"]
# Loop.
for element in list:
    m = re.match("(r\w+)\W(r\w+)", element)
if m:
        print(m.groups())
# Output
('red', 'rose')
('ruby', 'red')
```
**re.search(): Finding pattern in text**
The re.search() function will search the regular expression pattern and return the first occurrence. Unlike Python re.match(), it will check all lines of the input string. If the pattern is found, the match object will be returned, otherwise “null” is returned.
```
patterns=['penguin','Rosa']
aquarium_short="Because of problems with her eyesight, rey the African penguin had issues with swimming."
for pattern in patterns:
    print('Looking for "%s" in "%s" = '% (pattern, aquarium_short), end=" ")
    
    if re.search(pattern, aquarium_short):
        print("Match was found")
    else:
        print("No match was found")
# Output
Looking for "penguin" in "Because of problems with her eyesight, rey the African penguin had issues with swimming." =  Match was found
Looking for "Rosa" in "Because of problems with her eyesight, rey the African penguin had issues with swimming." =  No match was found
```

In this example, we looked for two strings, “penguin” and “Rosa” in a text string “Because of problems with her eyesight, rey the African penguin had issues with swimming.” For “penguin” we found the match hence it returns the output “Match was found”, while the word “Rosa” was not found in the string and returns “No match was found.”
