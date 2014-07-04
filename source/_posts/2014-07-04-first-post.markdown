---
layout: post
title: "First Post"
date: 2014-07-04 17:13:09 -0400
comments: true
categories: 
---

###Why the eternal white belt, you ask?

Without getting overly philosophical, I think it's really all about leaving your ego at the door and being ready to learn like a beginner. The white belt is a powerful tool - it allows you to know that you're not stupid, not completely incompetent, but acknowledge you may know nothing about a particular discipline. Yet. And that's completely ok. It helps keep things fresh; I think the best default state is to be learning.

<!-- More -->

Here's a quote from _Mastery_, a fantastic book by George Leonard. He closes his work with a story of Jigoro Kana, the man who both invented Judo and started the practice of wearing black and white belts in martial arts. He, perhaps more than anyone else, would understand the symbolic importance of the novice’s white belt:

>When [he] was quite old and close to death, the story goes, he called his students around him and told them we wanted to be buried in his white belt. What a touching story; how humble of the world’s highest-ranking judoist in his last days to ask for the emblem of a beginner! But Kano’s request, I eventually realized, was less humility than realism. At the moment of death, the ultimate transformation, we are all white belts. And if death makes beginners of us, so does life — again and again. In the master’s secret mirror, even at the moment of highest renown and accomplishment, there is an image of the newest student in the class, eager for knowledge, willing to play the fool.

Anyway, that's kinda cool.

And now:


->_"Get Jazzy On Me"_ - Jason Derulo <-

##Jazz Model

So we've been learning about ActiveRecord here at Flatiron School, and how it's a really awesome ORM that makes things much easier for us in developing applications. I've also known for a while that there is a connection between music theory and programming, especially with Python, and I wondered if there were a way to combine it with Ruby.

Jazz Model is an ActiveRecord model of concepts in Jazz theory, establishing relationships between chords and scales, and much more.

The core of Jazz Toolbox is a full Ruby object model representing concepts of Jazz theory
All chord/scale/mode/etc. definitions are stored as a mathematical system (sequences of numbers) which are then used to perform calculations.

For example, putting some chord in a different key is a matter of adding an arbitrary delta of half-steps and doing modulo 12.

###The Jazz Model Module

Their classes often inherit from their base Jazz Module.

```ruby
module JazzModel
  class Base < ActiveRecord::Base
    self.abstract_class = true
   
    establish_connection :adapter => "sqlite3", :database => ":memory:"
    load File.join(File.dirname(__FILE__), "../../db/schema.rb")
   
    def self.load_definitions(definition_name = :default)
      definition = JazzModel::Definition[definition_name]
      raise ArgumentError, "Definition #{definition_name} not found." unless definition
     
      definition.load
    end
  end
end
```
We can see that the module inherits from ActiveRecord, and that it uses a sqlite3 adapter by default. However, we learned that ActiveRecord is relational database agnostic. Definitions are things like, chords, scales, etc. And here's an example of a class - the Chord class!

```ruby
  class Chord < JazzModel::Base
    include KeyContext
   
    acts_as_tree
   
    belongs_to :chord_quality #zOMG we learned this yesterday
 
    has_many :symbols, :class_name => 'ChordSymbol', :extend => ChordSymbolCollection
    has_one :primary_symbol, :class_name => 'ChordSymbol', :conditions => {:primary => true}
 
    has_many :chord_scales
    has_many :modes, :through => :chord_scales
    has_many :tones, :class_name => 'ChordTone', :extend => ToneSequence
    has_many :voicings

    #a lot more stuff
  end #after that stuff

```
We have associations happenin', yo. In our study of ActiveRecord so far, we've learned that objects can be related to one another; namely, a chord has many tones (makes sense), a chord has many voicings (these are inversions, a version of the chord that uses the same notes but in a different arrangement and note in the bass of the chord. But we can also see that it belongs_to chord quality - namely, there's a class called ChordQuality, and we can see a bit of it here:

```ruby
  class ChordQuality < JazzModel::Base
    has_many :chords #<- #Chords, I like you too.
    class << self
      def resolve(name)
        self.find_by_name(name)
      end
      alias_method :[], :resolve
    end

  end
```
The relationship is reciprocated! And here's a quick look at what's going on in the database - this is a very truncated view and only represents the creation of the chords table!

```ruby
JazzModel::Base.connection.tap do |c|

  c.create_table :chord_qualities do |t|
    t.string :name
    t.string :code
  end
 
  c.create_table :chords do |t|
    t.belongs_to :chord_quality
    t.integer :parent_id
    t.string :name
    t.text :synonyms
    t.text :information
  end
```
We can also see a nice implementation of the tap method in our table creation. Below, we can see some of the usages of the method notes(), which returns an array of the notes of a chord. For music theory buffs, notice that it returns the correctly-spelled notes, not just the enharmonically easier ones!

```ruby
#Enumerate notes of a Chord:
Chord['maj'].notes   # Defaults to C without specified key context#
#=> ['C', 'E', 'G']

Chord['Ebmaj7'].notes
#=> ['Eb', 'G', 'Bb', 'D']

# Or specify key context with chained methods like this...
Chord['Bmaj7#11'].notes
#=> ['B', 'D#', 'F#', 'A#', 'E#']# Note E# - Correct theoretic value for this chord, not F
```
Who knew there would be a "notes" method for chords in Ruby? I sure didn't. And you can solve problems like these:

```ruby
#Ruby Example Problem:
#Find all chords associated with the Major scale and
#print each on a new line with the chord tones.

Scale['Major'].chords.map do |c|
c.name + ': ' + c.notes.join(', ')} * "\n"
# => Major 7: C, E, G, B    
#Major 6: C, E, G, A
#Dominant 6/9: C, E, G, Bb, D, A

```
Anyway, I think this could be a fun open-source project to contribute to.

Cheers,

Will
