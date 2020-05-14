# Language Integrated Query (LINQ)
Readings .NET 401d1 class 9

 technologies based on the integration of query capabilities directly into the C# language. 
  - queries against data are expressed as simple strings without type checking at compile time or IntelliSense support.
  -  Furthermore, you have to learn a different query language for each type of data source: SQL databases, XML documents, various Web services, and so on
  - LINQ, a query is a first-class language construct, just like classes, methods, events.

  - For a developer who writes queries, the most visible "language-integrated" part of LINQ is the query expression. Query expressions are written in a declarative query syntax.
  - You can write LINQ queries in C# for SQL Server databases, XML documents, ADO.NET Datasets, and any collection of objects that supports IEnumerable or the generic IEnumerable<T> interface. ou can write LINQ queries in C# for SQL Server databases, XML documents, ADO.NET Datasets, and any collection of objects that supports IEnumerable or the generic IEnumerable<T> interface. 

    The following example shows the complete query operation. The complete operation includes creating a data source, defining the query expression, and executing the query in a foreach statement.

        class LINQQueryExpressions
        {
             static void Main()
             {

                    // Specify the data source.
                    int[] scores = new int[] { 97, 92, 81, 60 };

                     // Define the query expression.
                      IEnumerable<int> scoreQuery =
                         from score in scores
                         where score > 80
                        select score;

                      // Execute the query.
                     foreach (int i in scoreQuery)
                      {
                        Console.Write(i + " ");
                      }
              }
        }
        // Output: 97 92 81
