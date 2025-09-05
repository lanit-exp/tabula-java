# Fork Tabula-Java

Форк библиотеки [tabula-java](https://github.com/tabulapdf/tabula-java), предназначенный для извлечения таблиц из PDF-документов.

## Отличия от оригинала

- Удалён лишний функционал:
    - `CommandLineApp`
    - тесты
    - неиспользуемый код
- Минимизирован набор зависимостей
- Обновлены версии используемых библиотек до последних
- Сохранена совместимость с **Java 8**

## О проекте

Этот форк предназначен для случаев, когда требуется только ядро распознавания таблиц без дополнительных утилит и CLI, что делает библиотеку более лёгкой и удобной для интеграции в сторонние проекты.

---


## Пример использования

Извлечение таблиц из PDF-документа постранично.

```java
public static List<List<Table>> getTablesFromPdf(String pdfFilePath) throws Exception {
    try (PDDocument document = Loader.loadPDF(new File(pdfFilePath))) {
        SpreadsheetExtractionAlgorithm ea = new SpreadsheetExtractionAlgorithm(); // или new BasicExtractionAlgorithm()

        List<List<Table>> result = new ArrayList<>();

        PageIterator pi = new ObjectExtractor(document).extract();

        while (pi.hasNext()) {
            Page page = pi.next();
            result.add(ea.extract(page));
        }
        return result;
    }
}
```

## Доступные алгоритмы извлечения

### 1. SpreadsheetExtractionAlgorithm()
- Подходит для PDF, созданных из табличных редакторов (Excel, Google Sheets и др.).
- Использует «сеточную» структуру для распознавания таблицы.
- Отличается высокой точностью, если документ содержит чётко выровненные столбцы и строки.
- Рекомендуется для машинно-сгенерированных PDF.

### 2. BasicExtractionAlgorithm()
- Универсальный алгоритм для поиска таблиц в PDF.
- Основан на анализе расположения текста и промежутков между словами.
- Может извлекать таблицы даже из документов без сетки (например, отчётов или документов, где таблица напечатана как текст).
- Работает хуже на «чистых» Excel-таблицах по сравнению с Spreadsheet-алгоритмом, но зато справляется с менее структурированными таблицами.

