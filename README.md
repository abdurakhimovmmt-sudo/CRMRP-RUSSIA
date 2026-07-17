# CRMRP-RUSSIA
OFFICIAL SITE
# Импортируем необходимые модули
import os

class FileManager:
    def __init__(self, directory='files'):
        # Создаем директорию для файлов, если её нет
        if not os.path.exists(directory):
            os.makedirs(directory)
        self.directory = directory

    def create_file(self, filename, content=''):
        """Создание нового файла"""
        filepath = os.path.join(self.directory, filename)
        try:
            with open(filepath, 'w') as file:
                file.write(content)
            print(f"Файл {filename} создан успешно")
        except Exception as e:
            print(f"Ошибка при создании файла: {str(e)}")

    def read_file(self, filename):
        """Чтение файла"""
        filepath = os.path.join(self.directory, filename)
        try:
            with open(filepath, 'r') as file:
                content = file.read()
            print(f"Содержимое файла {filename}:")
            return content
        except FileNotFoundError:
            print(f"Файл {filename} не найден")
        except Exception as e:
            print(f"Ошибка при чтении файла: {str(e)}")

    def append_to_file(self, filename, content):
        """Добавление данных в конец файла"""
        filepath = os.path.join(self.directory, filename)
        try:
            with open(filepath, 'a') as file:
                file.write(content)
            print(f"Данные добавлены в файл {filename}")
        except FileNotFoundError:
            print(f"Файл {filename} не найден")
        except Exception as e:
            print(f"Ошибка при записи в файл: {str(e)}")

    def delete_file(self, filename):
        """Удаление файла"""
        filepath = os.path.join(self.directory, filename)
        try:
            os.remove(filepath)
            print(f"Файл {filename} удален успешно")
        except FileNotFoundError:
            print(f"Файл {filename} не найден")
        except Exception as e:
            print(f"Ошибка при удалении файла: {str(e)}")

# Пример использования
if __name__ == "__main__":
    manager = FileManager()
    
    # Создаем файл
    manager.create_file('example.txt', 'Привет, мир!')
    
    # Читаем файл
    print(manager.read_file('example.txt'))
    
    # Добавляем данные
    manager.append_to_file('example.txt', '\nЭто новая строка.')
    
    # Читаем обновленный файл
    print(manager.read_file('example.txt'))
    
    # Удаляем файл
    manager.delete_file('example.txt')
