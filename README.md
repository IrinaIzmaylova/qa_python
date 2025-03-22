# qa_python
import pytest
from main import BooksCollector 

class TestBooksCollector:
    @pytest.fixture
    def books_collector(self):
        
        return BooksCollector()

    def test_add_new_book(self, books_collector):
        
        books_collector.add_new_book("Война и мир")
        assert "Война и мир" in books_collector.get_books_genre()

    def test_add_new_book_with_invalid_name(self, books_collector):
        
        books_collector.add_new_book("a" * 41)
        assert len(books_collector.get_books_genre()) == 0

    def test_add_duplicate_book(self, books_collector):
        
        books_collector.add_new_book("1984")
        books_collector.add_new_book("1984")  
        assert len(books_collector.get_books_genre()) == 1

    def test_set_book_genre(self, books_collector):
        
        books_collector.add_new_book("451 градус по Фаренгейту")
        books_collector.set_book_genre("451 градус по Фаренгейту", "Фантастика")
        assert books_collector.get_book_genre("451 градус по Фаренгейту") == "Фантастика"

    def test_get_books_with_specific_genre(self, books_collector):
        
        books_collector.add_new_book("Сияние")
        books_collector.set_book_genre("Сияние", "Ужасы")
        assert books_collector.get_books_with_specific_genre("Ужасы") == ["Сияние"]

    def test_get_books_for_children(self, books_collector):
        
        books_collector.add_new_book("Книга для детей")
        books_collector.set_book_genre("Книга для детей", "Детективы")
        assert "Книга для детей" not in books_collector.get_books_for_children()

    def test_add_book_in_favorites(self, books_collector):
        
        books_collector.add_new_book("Мастер и Маргарита")
        books_collector.set_book_genre("Мастер и Маргарита", "Фантастика")
        books_collector.add_book_in_favorites("Мастер и Маргарита")
        assert "Мастер и Маргарита" in books_collector.get_list_of_favorites_books()

    def test_delete_book_from_favorites(self, books_collector):
        
        books_collector.add_new_book("Гарри Поттер")
        books_collector.set_book_genre("Гарри Поттер", "Фантастика")
        books_collector.add_book_in_favorites("Гарри Поттер")
        books_collector.delete_book_from_favorites("Гарри Поттер")
        assert "Гарри Поттер" not in books_collector.get_list_of_favorites_books()

    @pytest.mark.parametrize("book_name, expected", [
        ("Книга 1", "Фантастика"),
        ("Книга 2", "Ужасы"),
        ("Книга 3", "Детективы"),
    ])
    def test_parametrized_set_book_genre(self, books_collector, book_name, expected):
        
        books_collector.add_new_book(book_name)
        books_collector.set_book_genre(book_name, expected)
        assert books_collector.get_book_genre(book_name) == expected
>>>>>>> a516cb7 (Initial commit)
