# TomeRater
Final project for class.


class User(object):
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.books = {}

    def get_email(self):
        return self.email

    def change_email(self, address):
        self.email = address
        return "The users email has been changed to " + self.email

    def __repr__(self):
        return " User: {}, email: {}, books read = {}".format(self.name, self.email, self.books)


    def __eq__(self, other_user):
        return self.name == other_user.name and self.email == other_user.email

    def read_book(self, book, rating):
  	    self.books[book] = rating

    def get_average_rating(self):
       avg_rate = 0
       count = 0
       for v in self.books.values():
           if v != None:
             avg_rate += v
             count += 1
       return avg_rate / count

class Book(object):

    def __init__(self, title, isbn, price):
      self.title = title
      self.isbn = isbn
      self.ratings = []
      self.price = price
      
    def __repr__(self):
        return "{}".format(self.title)


    def get_title(self):
        return self.title

    def get_isbn(self):
        return self.isbn
    
    def get_price(self):
        return self.price

    def set_isbn(self, new_isbn):
        self.isbn = new_isbn
        return "this book’s ISBN has been updated."

    def add_rating(self, rating):
          if rating is not None and rating >= 0 and rating <= 4:
            self.ratings.append(rating)
          else:
            return "Invalid Rating"

    def get_average_rating(self):
        total = 0
        for i in self.ratings:
            total += i
        return total/len(self.ratings)

    def __eq__(self, other_book):
        return self.title == other_book.title and self.isbn == other_book.isbn

    def __hash__(self):
        return hash((self.title, self.isbn))


class Fiction(Book):

    def __init__(self, title, author, isbn, price):
        super().__init__(title, isbn, price)
        self.author = author

    def get_author(self):
        return self.author

    def __repr__(self):
        return "{} by  {}".format(self.title, self.author)


class NonFiction(Book):

    def __init__(self, title, subject, level, isbn, price):
        super().__init__(title, isbn, price)
        self.subject = subject
        self.level = level

    def get_subject(self):
        return self.subject

    def get_level(self):
        return self.level

    def __repr__(self):
        return "{}, a {} manual on {}".format(self.title, self.level, self.subject)



class TomeRater:

    def __init__(self):
        self.users = {}
        self.books = {}
        self.prices = {}


    def create_book(self, title, isbn, price):
        return Book(title, isbn, price)

    def create_novel(self, title, author, isbn, price):
        return Fiction(title, author, isbn, price)

    def create_non_fiction(self, title, subject, level, isbn, price):
        return NonFiction(title, subject, level, isbn, price)

    def add_book_to_user(self, book, email, rating = None):
        if email not in self.users:
            print("No user with email {}".format(email))
        else:
          self.users[email].read_book(book, rating)
          book.add_rating(rating)
        if book not in self.books:
            self.books[book] = 1
        else:
            self.books[book] += 1
            
    def add_user(self, name, email, books = None):
        user = User(name, email)
        self.users[email] = user
        if books != None:
            for book in books:
                self.add_book_to_user(book, email)
                
    def print_catalog(self):
        catalog = self.books.keys()
        print (catalog)
    
    def print_users(self):
        for v in self.users.values():
            print(v)
            
    def most_read_book(self):
        maximum = max(self.books, key = self.books.get)
        print("{} has been read {} times.".format(maximum, self.books[maximum]))
        
    def highest_rated_book(self):
        ratings = {}
        for book in self.books:
            ratings[book] = book.get_average_rating()
        maximum = max(ratings, key = ratings.get)
        print("Book: {}\nScore: {}".format(maximum, ratings[maximum]))
        
    def most_positive_user(self):
        ratings = {}
        for user in self.users.values():
            ratings[user.name] = user.get_average_rating()
        maximum = max(ratings, key = ratings.get)
        print("User: {}\nScore: {}".format(maximum, ratings[maximum]))
        
        
    def price_of_book(self, book):
        if book in self.books:
            print(book.price)
            
            
    def most_expensive_books(self, n):
        new_dict = {}
        for key in self.books:
            new_dict[key] = key.get_price()
        for i in range(0, n):
            i = max(new_dict, key = new_dict.get)
            print("{}    Price: $%.2f".format(i) % new_dict[i])
            del new_dict[i]
            

        
            
            
            

            
            
        
            
        
        
            
    
Tome_Rater = TomeRater()

#Create some books:
book1 = Tome_Rater.create_book("Society of Mind", 12345678, 8.87)
novel1 = Tome_Rater.create_novel("Alice In Wonderland", "Lewis Carroll", 12345, 9.79)
novel1.set_isbn(9781536831139)
nonfiction1 = Tome_Rater.create_non_fiction("Automate the Boring Stuff", "Python", "beginner", 1929452, 6.99)
nonfiction2 = Tome_Rater.create_non_fiction("Computing Machinery and Intelligence", "AI", "advanced", 11111938, 23.20)
novel2 = Tome_Rater.create_novel("The Diamond Age", "Neal Stephenson", 10101010, 11.64)
novel3 = Tome_Rater.create_novel("There Will Come Soft Rains", "Ray Bradbury", 10001000, 2)

#Create users:
Tome_Rater.add_user("Alan Turing", "alan@turing.com")
Tome_Rater.add_user("David Marr", "david@computation.org")

#Add a user with three books already read:
Tome_Rater.add_user("Marvin Minsky", "marvin@mit.edu", books=[book1, novel1, nonfiction1])

#Add books to a user one by one, with ratings:
Tome_Rater.add_book_to_user(book1, "alan@turing.com", 1)
Tome_Rater.add_book_to_user(novel1, "alan@turing.com", 3)
Tome_Rater.add_book_to_user(nonfiction1, "alan@turing.com", 3)
Tome_Rater.add_book_to_user(nonfiction2, "alan@turing.com", 4)
Tome_Rater.add_book_to_user(novel3, "alan@turing.com", 1)

Tome_Rater.add_book_to_user(novel2, "marvin@mit.edu", 2)
Tome_Rater.add_book_to_user(novel3, "marvin@mit.edu", 2)
Tome_Rater.add_book_to_user(novel3, "david@computation.org", 4)


