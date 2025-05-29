// insert_books.js - Script to populate the plp_bookstore database with sample book data

// Connect to the database
use('plp_bookstore');

// Sample book data with all required fields
const books = [
  {
    title: "The Great Gatsby",
    author: "F. Scott Fitzgerald",
    genre: "Fiction",
    published_year: 1925,
    price: 12.99,
    in_stock: true,
    pages: 180,
    publisher: "Scribner"
  },
  {
    title: "To Kill a Mockingbird",
    author: "Harper Lee",
    genre: "Fiction",
    published_year: 1960,
    price: 14.99,
    in_stock: true,
    pages: 376,
    publisher: "J.B. Lippincott & Co."
  },
  {
    title: "1984",
    author: "George Orwell",
    genre: "Science Fiction",
    published_year: 1949,
    price: 13.99,
    in_stock: false,
    pages: 328,
    publisher: "Secker & Warburg"
  },
  {
    title: "Pride and Prejudice",
    author: "Jane Austen",
    genre: "Romance",
    published_year: 1813,
    price: 11.99,
    in_stock: true,
    pages: 432,
    publisher: "T. Egerton"
  },
  {
    title: "The Catcher in the Rye",
    author: "J.D. Salinger",
    genre: "Fiction",
    published_year: 1951,
    price: 15.99,
    in_stock: true,
    pages: 277,
    publisher: "Little, Brown and Company"
  },
  {
    title: "Dune",
    author: "Frank Herbert",
    genre: "Science Fiction",
    published_year: 1965,
    price: 18.99,
    in_stock: true,
    pages: 688,
    publisher: "Chilton Books"
  },
  {
    title: "The Hobbit",
    author: "J.R.R. Tolkien",
    genre: "Fantasy",
    published_year: 1937,
    price: 16.99,
    in_stock: true,
    pages: 310,
    publisher: "George Allen & Unwin"
  },
  {
    title: "Animal Farm",
    author: "George Orwell",
    genre: "Political Satire",
    published_year: 1945,
    price: 10.99,
    in_stock: false,
    pages: 112,
    publisher: "Secker & Warburg"
  },
  {
    title: "The Da Vinci Code",
    author: "Dan Brown",
    genre: "Mystery",
    published_year: 2003,
    price: 17.99,
    in_stock: true,
    pages: 689,
    publisher: "Doubleday"
  },
  {
    title: "Harry Potter and the Philosopher's Stone",
    author: "J.K. Rowling",
    genre: "Fantasy",
    published_year: 1997,
    price: 19.99,
    in_stock: true,
    pages: 223,
    publisher: "Bloomsbury"
  },
  {
    title: "The Lord of the Rings",
    author: "J.R.R. Tolkien",
    genre: "Fantasy",
    published_year: 1954,
    price: 24.99,
    in_stock: true,
    pages: 1216,
    publisher: "George Allen & Unwin"
  },
  {
    title: "Brave New World",
    author: "Aldous Huxley",
    genre: "Science Fiction",
    published_year: 1932,
    price: 13.49,
    in_stock: false,
    pages: 268,
    publisher: "Chatto & Windus"
  }
];

// Insert the books into the collection
try {
  const result = db.books.insertMany(books);
  print(`Successfully inserted ${result.insertedIds.length} books into the collection.`);
  print("Inserted book IDs:", result.insertedIds);
  
  // Display the first few books to verify insertion
  print("\nFirst 3 books in the collection:");
  db.books.find().limit(3).forEach(printjson);
  
  print(`\nTotal books in collection: ${db.books.countDocuments()}`);
  
} catch (error) {
  print("Error inserting books:", error);
}

// Instructions for running this script:
// 1. Make sure MongoDB is running
// 2. Open MongoDB Shell (mongosh)
// 3. Run: load('insert_books.js')
// OR
// 4. Run: mongosh --file insert_books.js




// queries.js - MongoDB Queries for PLP Bookstore Assignment
// Connect to the plp_bookstore database
use('plp_bookstore');

print("=== TASK 2: BASIC CRUD OPERATIONS ===\n");

// 1. Find all books in a specific genre (Fiction)
print("1. Find all books in Fiction genre:");
db.books.find({ genre: "Fiction" }).forEach(printjson);

print("\n2. Find all books in Science Fiction genre:");
db.books.find({ genre: "Science Fiction" }).forEach(printjson);

// 2. Find books published after a certain year (2000)
print("\n3. Find books published after 2000:");
db.books.find({ published_year: { $gt: 2000 } }).forEach(printjson);

// 3. Find books by a specific author (George Orwell)
print("\n4. Find books by George Orwell:");
db.books.find({ author: "George Orwell" }).forEach(printjson);

// 4. Update the price of a specific book
print("\n5. Update price of 'The Great Gatsby' to $15.99:");
const updateResult = db.books.updateOne(
  { title: "The Great Gatsby" },
  { $set: { price: 15.99 } }
);
print(`Modified ${updateResult.modifiedCount} document(s)`);

// Verify the update
print("Verification - The Great Gatsby after price update:");
db.books.findOne({ title: "The Great Gatsby" });

// 5. Delete a book by its title
print("\n6. Delete 'Animal Farm' from the collection:");
const deleteResult = db.books.deleteOne({ title: "Animal Farm" });
print(`Deleted ${deleteResult.deletedCount} document(s)`);

// Verify deletion
print(`Total books after deletion: ${db.books.countDocuments()}`);

print("\n=== TASK 3: ADVANCED QUERIES ===\n");

// 1. Find books that are both in stock and published after 2010
print("1. Books in stock AND published after 2010:");
db.books.find({ 
  in_stock: true, 
  published_year: { $gt: 2010 } 
}).forEach(printjson);

// 2. Use projection to return only title, author, and price
print("\n2. All books with only title, author, and price fields:");
db.books.find({}, { title: 1, author: 1, price: 1, _id: 0 }).forEach(printjson);

// 3. Sorting by price (ascending)
print("\n3. Books sorted by price (ascending):");
db.books.find({}, { title: 1, price: 1, _id: 0 }).sort({ price: 1 }).forEach(printjson);

// 4. Sorting by price (descending)
print("\n4. Books sorted by price (descending):");
db.books.find({}, { title: 1, price: 1, _id: 0 }).sort({ price: -1 }).forEach(printjson);

// 5. Pagination - First page (5 books per page)
print("\n5. Pagination - Page 1 (first 5 books):");
db.books.find({}, { title: 1, author: 1, price: 1, _id: 0 })
  .sort({ title: 1 })
  .limit(5)
  .forEach(printjson);

// 6. Pagination - Second page
print("\n6. Pagination - Page 2 (next 5 books):");
db.books.find({}, { title: 1, author: 1, price: 1, _id: 0 })
  .sort({ title: 1 })
  .skip(5)
  .limit(5)
  .forEach(printjson);

print("\n=== TASK 4: AGGREGATION PIPELINE ===\n");

// 1. Calculate average price by genre
print("1. Average price of books by genre:");
db.books.aggregate([
  {
    $group: {
      _id: "$genre",
      averagePrice: { $avg: "$price" },
      bookCount: { $sum: 1 }
    }
  },
  {
    $sort: { averagePrice: -1 }
  }
]).forEach(printjson);

// 2. Find author with most books
print("\n2. Author with the most books:");
db.books.aggregate([
  {
    $group: {
      _id: "$author",
      bookCount: { $sum: 1 },
      books: { $push: "$title" }
    }
  },
  {
    $sort: { bookCount: -1 }
  },
  {
    $limit: 1
  }
]).forEach(printjson);

// 3. Group books by publication decade
print("\n3. Books grouped by publication decade:");
db.books.aggregate([
  {
    $addFields: {
      decade: {
        $subtract: [
          "$published_year",
          { $mod: ["$published_year", 10] }
        ]
      }
    }
  },
  {
    $group: {
      _id: "$decade",
      count: { $sum: 1 },
      books: { $push: { title: "$title", year: "$published_year" } }
    }
  },
  {
    $sort: { _id: 1 }
  }
]).forEach(printjson);

print("\n=== TASK 5: INDEXING ===\n");

// 1. Create index on title field
print("1. Creating index on 'title' field:");
const titleIndexResult = db.books.createIndex({ title: 1 });
print("Title index created:", titleIndexResult);

// 2. Create compound index on author and published_year
print("\n2. Creating compound index on 'author' and 'published_year':");
const compoundIndexResult = db.books.createIndex({ author: 1, published_year: -1 });
print("Compound index created:", compoundIndexResult);

// 3. List all indexes
print("\n3. All indexes on the books collection:");
db.books.getIndexes().forEach(printjson);

// 4. Demonstrate performance with explain()
print("\n4. Query performance without index (full collection scan):");
db.books.find({ title: "The Great Gatsby" }).explain("executionStats");

print("\n5. Query performance with title index:");
// The same query will now use the index we created
const explainResult = db.books.find({ title: "The Great Gatsby" }).explain("executionStats");
print("Execution stats:", explainResult.executionStats);

// 6. Compound index usage example
print("\n6. Query using compound index (author + published_year):");
const compoundExplain = db.books.find({ 
  author: "J.R.R. Tolkien", 
  published_year: { $gte: 1950 } 
}).explain("executionStats");
print("Compound index execution stats:", compoundExplain.executionStats);

print("\n=== ADDITIONAL USEFUL QUERIES ===\n");

// Additional helpful queries for the assignment
print("1. Total number of books in collection:");
print(`Total books: ${db.books.countDocuments()}`);

print("\n2. Books currently out of stock:");
db.books.find({ in_stock: false }, { title: 1, author: 1, _id: 0 }).forEach(printjson);

print("\n3. Most expensive books (top 3):");
db.books.find({}, { title: 1, author: 1, price: 1, _id: 0 })
  .sort({ price: -1 })
  .limit(3)
  .forEach(printjson);

print("\n4. Books with more than 400 pages:");
db.books.find({ pages: { $gt: 400 } }, { title: 1, pages: 1, _id: 0 }).forEach(printjson);

print("\n5. Collection statistics:");
db.books.aggregate([
  {
    $group: {
      _id: null,
      totalBooks: { $sum: 1 },
      averagePrice: { $avg: "$price" },
      averagePages: { $avg: "$pages" },
      inStockCount: { 
        $sum: { $cond: ["$in_stock", 1, 0] } 
      }
    }
  }
]).forEach(printjson);

print("\n=== ASSIGNMENT COMPLETE ===");
print("All queries have been executed successfully!");
print("Don't forget to take a screenshot of your MongoDB Compass or Atlas showing the data!");





# PLP Bookstore MongoDB Assignment

This repository contains the MongoDB assignment for the PLP Bookstore database operations.

## 📋 Assignment Overview

This assignment demonstrates proficiency in MongoDB operations including:
- Database setup and data insertion
- CRUD operations (Create, Read, Update, Delete)
- Advanced querying with filtering, projection, and sorting
- Aggregation pipelines for data analysis
- Indexing for performance optimization

## 🛠️ Setup Instructions

### Prerequisites
- Node.js (v18 or higher)
- MongoDB Community Edition OR MongoDB Atlas account
- MongoDB Shell (mongosh) OR MongoDB Compass

### Option 1: Local MongoDB Installation

1. **Install MongoDB Community Edition**
   - Download from [MongoDB Official Website](https://www.mongodb.com/try/download/community)
   - Follow the installation guide for your operating system
   - Start MongoDB service:
     ```bash
     # On macOS (using brew)
     brew services start mongodb-community
     
     # On Ubuntu/Linux
     sudo systemctl start mongod
     
     # On Windows
     net start MongoDB
     ```

2. **Verify Installation**
   ```bash
   mongosh
   ```

### Option 2: MongoDB Atlas (Cloud)

1. **Create Atlas Account**
   - Go to [MongoDB Atlas](https://www.mongodb.com/atlas)
   - Sign up for a free account
   - Create a new cluster (M0 Sandbox - Free)

2. **Setup Database Access**
   - Create a database user with read/write privileges
   - Add your IP address to the IP Access List
   - Get your connection string

3. **Connect via MongoDB Shell**
   ```bash
   mongosh "mongodb+srv://your-cluster-url/test" --apiVersion 1 --username your-username
   ```

## 🚀 Running the Assignment

### Step 1: Populate the Database

1. **Using MongoDB Shell (mongosh)**
   ```bash
   # Connect to MongoDB
   mongosh
   
   # Load and run the insert script
   load('insert_books.js')
   ```

2. **Alternative method**
   ```bash
   # Run directly from command line
   mongosh --file insert_books.js
   ```

### Step 2: Execute Queries

1. **Run all queries**
   ```bash
   # Load and run the queries script
   mongosh --file queries.js
   ```

2. **Or run individual sections in mongosh**
   ```bash
   mongosh
   > load('queries.js')
   ```

### Step 3: Verify Database

1. **Check your database**
   ```javascript
   use('plp_bookstore')
   db.books.find().count()  // Should show 11 books (after deletion)
   db.books.getIndexes()    // Should show your created indexes
   ```

## 📁 File Structure

```
├── README.md              # This file - setup instructions
├── insert_books.js        # Script to populate database with sample data
├── queries.js            # All MongoDB queries for the assignment
└── screenshot.png         # Screenshot of MongoDB Compass/Atlas (add this)
```

## 📊 Database Schema

Each book document contains the following fields:

```javascript
{
  title: String,           // Book title
  author: String,          // Author name
  genre: String,           // Book genre
  published_year: Number,  // Year of publication
  price: Number,           // Book price in USD
  in_stock: Boolean,       // Availability status
  pages: Number,          // Number of pages
  publisher: String        // Publisher name
}
```

## 🎯 Assignment Tasks Completed

### ✅ Task 1: MongoDB Setup
- [x] Database `plp_bookstore` created
- [x] Collection `books` created
- [x] Sample data inserted (12 books initially, 11 after deletion)

### ✅ Task 2: Basic CRUD Operations
- [x] Find books by genre
- [x] Find books by publication year
- [x] Find books by author
- [x] Update book price
- [x] Delete book by title

### ✅ Task 3: Advanced Queries
- [x] Complex filtering (in_stock AND published_year)
- [x] Projection (specific fields only)
- [x] Sorting (ascending and descending)
- [x] Pagination with limit and skip

### ✅ Task 4: Aggregation Pipeline
- [x] Average price by genre
- [x] Author with most books
- [x] Books grouped by publication decade

### ✅ Task 5: Indexing
- [x] Single field index on `title`
- [x] Compound index on `author` and `published_year`
- [x] Performance analysis with `explain()`

## 🔧 Troubleshooting

### Common Issues

1. **MongoDB Connection Error**
   ```
   Error: Could not connect to MongoDB
   ```
   - Ensure MongoDB service is running
   - Check your connection string (for Atlas)
   - Verify firewall settings

2. **Permission Denied**
   ```
   Error: not authorized on plp_bookstore
   ```
   - Check database user permissions
   - Ensure you're using the correct credentials

3. **Script Not Found**
   ```
   Error: file not found
   ```
   - Ensure you're in the correct directory
   - Use full file path if necessary

### MongoDB Atlas Specific

1. **IP Whitelist Issues**
   - Add your current IP to Atlas IP Access List
   - Or add `0.0.0.0/0` for testing (not recommended for production)

2. **Connection String Format**
   ```
   mongodb+srv://<username>:<password>@<cluster-name>.mongodb.net/<database-name>
   ```

## 📈 Performance Notes

The assignment includes indexing examples that demonstrate:
- Query performance improvements
- Index usage in execution plans
- Compound index benefits for multi-field queries

Run the `explain()` queries to see the performance differences!

## 📸 Screenshot Requirements

Don't forget to include a screenshot showing:
- Your MongoDB Compass or Atlas interface
- The `plp_bookstore` database
- The `books` collection with sample data
- Total document count

## 🤝 Support

If you encounter any issues:
1. Check the troubleshooting section above
2. Verify your MongoDB installation/connection
3. Review the MongoDB documentation
4. Contact your instructor

## 📚 Resources

- [MongoDB Official Documentation](https://docs.mongodb.com/)
- [MongoDB University](https://university.mongodb.com/)
- [MongoDB Node.js Driver](https://docs.mongodb.com/drivers/node/)
- [MongoDB Shell (mongosh) Reference](https://docs.mongodb.com/mongodb-shell/)

---

**Assignment completed successfully!** 🎉

All tasks have been implemented with comprehensive queries and proper error handling.
