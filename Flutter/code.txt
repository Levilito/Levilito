import 'package:flutter/material.dart';

class Book {
  final String language;
  final String title;
  final List<String> pages;

  Book({required this.language, required this.title, required this.pages});
}

class Bookshelf {
  final List<Book> books;

  Bookshelf({required this.books});
}

class Student {
  final String name;
  final Bookshelf bookshelf;

  Student({required this.name, required this.bookshelf});
}

class Lesson {
  final String name;
  final String content;

  Lesson({required this.name, required this.content});
}

class BookViewer extends StatefulWidget {
  final Book book;

  BookViewer({required this.book});

  @override
  _BookViewerState createState() => _BookViewerState();
}

class _BookViewerState extends State<BookViewer> {
  int _currentPageIndex = 0;

  void _changePage(int index) {
    setState(() {
      _currentPageIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.book.title),
      ),
      body: Column(
        children: [
          Expanded(
            child: Container(
              child: SingleChildScrollView(
                child: Text(
                  widget.book.pages[_currentPageIndex],
                  style: TextStyle(fontSize: 18.0),
                ),
              ),
            ),
          ),
          Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              IconButton(
                onPressed: _currentPageIndex > 0
                    ? () => _changePage(_currentPageIndex - 1)
                    : null,
                icon: Icon(Icons.arrow_back),
              ),
              Text('${_currentPageIndex + 1}/${widget.book.pages.length}'),
              IconButton(
                onPressed: _currentPageIndex < widget.book.pages.length - 1
                    ? () => _changePage(_currentPageIndex + 1)
                    : null,
                icon: Icon(Icons.arrow_forward),
              ),
            ],
          ),
        ],
      ),
    );
  }
}

class LessonSearch extends StatefulWidget {
  final List<Lesson> lessons;

  LessonSearch({required this.lessons});

  @override
  _LessonSearchState createState() => _LessonSearchState();
}

class _LessonSearchState extends State<LessonSearch> {
  String _query = '';

  void _updateQuery(String query) {
    setState(() {
      _query = query;
    });
  }

  @override
  Widget build(BuildContext context) {
    List<Lesson> filteredLessons = widget.lessons.where((lesson) =>
        lesson.name.toLowerCase().contains(_query.toLowerCase())).toList();

    return Scaffold(
      appBar: AppBar(
        title: Text('Search Lessons'),
      ),
      body: Column(
        children: [
          Padding(
            padding: EdgeInsets.all(16.0),
            child: TextField(
              onChanged: _updateQuery,
              decoration: InputDecoration(
                labelText: 'Search Lessons',
                prefixIcon: Icon(Icons.search),
              ),
            ),
          ),
          Expanded(
            child: ListView.builder(
              itemCount: filteredLessons.length,
              itemBuilder: (BuildContext context, int index) {
                return ListTile(
                  title: Text(filteredLessons[index].name),
                  onTap: () {
                    // navigate to book viewer for lesson
                  },
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}

class MyApp extends StatelessWidget {
  final List<Student> students = [
    Student(
      name: 'John',
      bookshelf: Bookshelf(
        books: [
          Book(
            language: 'Arabic
