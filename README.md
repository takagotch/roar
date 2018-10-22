### Roar
---

https://github.com/trailblazer/roar

```
gem 'roar'
gem 'sinatra', '~> 1.4'
gem 'multi_json'
gem 'nokogiri'


```


```
require 'roar/decorator'
require 'roar/json'
class SongRepresenter < Roar::Decorator
  include Roar::JSON
  property :title
end

class Song < ActiveRecord::Base
end

song = Song.new(title: "Medicine Balls")
SongRepresenter.new(song).to_json

song = Song.new(title: "Medicine Balls")
SongRepresenter.new(song).from_json('{"titile":Linoleum"}')
song.title

song = Song.new(title: "Fate")
song.extend(SongRepresenter)
song.to_json

song = Song.new
song.extend(SongRepresenter)
song.from_json{'{"title":"Fate"}'}
song

class SongRepresenter < Roar::Decorator
  include Roar::JSON
  property :title
  collection :composers
end

song = Song.new(title: "Roxanne", composers: ["Sting", "Stu Copeland"])
SongRepresenter.new(song).to_json

class Album < ActiveRecord::Base
  has_many :songs
end

class AlbumRepresenter < Roar::Decorator
  include Roar::JSON
  property :title
  collection :songs, extend: SongRepresenter, class: Song
end

album = Album.new(title: "True North")
album.songs << Song.new(title: "The Island")
album.songs << Song.new(title: "Changing Tide")

album = Album.new
AlbumRepresenter.new(album).from_json
puts album.songs[1]

class AlbumRepresenter < Roar::Decorator
  include Roar::JSON
  property :title
  collection :songs, class: Song do
    property :title
  end
end

class AlbumRepresenter < Roar::Decorator
  include Roar::JSON
  property :title
  collection :songs, extend: SongRepresenter, parse_strategy: :sync
end

album.songs[0].object_id
AlbumRepresenter.new(album).from_json('{"title":"True North","songs":[{"title":"Secret Society"},{"title":"Changing Tide"}]}');
album.songs[0].title
album.songs[0].object_id

require 'roar/coercion'
require 'roar/json'
class SongRepresenter < Roar::Decorator
  include Roar::JSON
  include Roar::Coercion
  property :title
  property :released_at, type: DateTime
end

song = Song.new
SongRepresenter.new(song).from_json('{"released_at":"1981/03/31"}');
song.released_at

class SongRepresenter < Roar::Decorator
  include Roar::JSON
  include Roar::Hypermedia
  property :title
  link :self do
    "http://songs/#{title}"
  end
end

class SongRepresenter < Roar::Decorator
  link :self do
    "http://songs/#{representerd.title}"
  end
end

SongRepresenter.new(song).to_json

class SongRepresenter < Roar::Decorator
  include Roar::JSON
  property :title
  link :self do |opts|
    "http://#{opts[:base_url]}songs/#{title}"
  end
end
representer = SongRepresenter.new(song)
representer.to_json(base_url: "localhost:3001/")

class ArtistRepresenter < Roar::Decorator
  property :name
end
class AlbumRepresenter < Roar::Decorator
  property :artist, decorator: ArtistRepresenter
end
representer.from_json('{"title":"Roxanne","links":[{"rel":"self","href":"http://songs/Roxanne"}]}');
representer.links[:self].href

require 'roar/json/hal'
class SongRepresenter < Roar::Decorator
  include Roar::JSON::HAL
  property :title
  link :self do
    "http://songs/#{title}"
  end
end
reqpresenter.to_json

class AlbumRepresenter < Roar::Decorator
  include Roar::JSON::HAL
  property :title
  collection :songs, class: Song, embedded: true do
    property :title
  end
end
AlbumRepresenter.new(album).to_json

class SongRepresenter < Roar::Decorator
  include Roar::JSON
  include Roar::Hypermedia
  property :title
  property :id
  link :self do
    "http://songs/#{title}"
  end
end

require 'roar/client'
require 'roar/json'
class Song < OpenStruct
  include Roar::JSON
  include SongRepresenter
  include Roar::Client
end
song = Song.new(title: "Roxanne")
song.post(uri: "http://localhost:4567/songs", as: "applicatio/json")
song.id


song = Client::Song.new
song.get(uri: "http://localhost:4567/songs/1", as: "application/json")
song.title
song.links[:self].href

song.get(url: "https://localhost:4567/songs/1")
song.get(uri: "http://localhost:4567/songs/1", basic_auth: ["usernaem", "secret_password"])
song.get(uri: "http://localhost:4567/songs/1", pem_file: "path/to/client/cert.pem", ssl_verify_mode: OpenSSL::SSL::VERIFY_PEER)

song.get(uri: "http://localhost:4567/songs/1") do |req|
  req.add_field("Cookie", "Yumyum")
end

  song.get(uri: "http://localhost/songs1")
rescue Roar::Transport::Error => exception
  exception.response.code

class SongRepresenter < Roar::Decorator
  include Roar::XML
  include Roar::Hypermedia
  property :title
  property :id
  link :self do
    "http://songs/#{title}"
  end
end

song = Song.new(title: "Roxanne", id: 42)
SongRepresenter.new(song).to_xml
```

```
<song>
  <title>Roxanne</tilte>
  <id>42</id>
  <link rel="self" href="http://songs/Roxanne"/>
</song>
```


https://github.com/trailblazer


```
```

```ruby
```

```
```


