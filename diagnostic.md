# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indicated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
  A join table, in a customers/library relationship for example, is valuable because the join table
  can contain just the information relevant to the relationship. A customers table likely has personal
  information that isn't pertinent to the customer interaction with the books in the library. Creating a join
  table, borrowers, allows us to have a table with data such as checkout date and due date but not excess info such as
  customer address.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    Profiles |----< Favorites >----| Movies

    Profile
    -profile_id
    -given_name
    -surname
    -email

    Movies
    -movie_id
    -title
    -release_date
    -length

    Favorites
    -favorites_id
    -movie_id (references to movie_id)
        --would contain multiple movie id's
    -profile_id(references to profile_id)
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites, dependent: destroy

  validates :given_name, presence: true
  validates :surname, presence: true
  validates :email, presence: true

  end
  ```

  ```rb
  class Movie < ActiveRecord::Base

  has_many :profiles, through: :favorites
  has_many :favorites, dependent: destroy

  validates :title, presence: true

  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to: movies, inverse_of: :favorites
    belongs_to :profiles, inverse_of: :favorites

  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    # < Your Response Here >
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
   attributes :id, :title, :release_date, :length
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    # < Your Response Here >
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
  it goes in the model and allows us to destroy an entry in the DB 
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    # < Your Response Here >
  ```
