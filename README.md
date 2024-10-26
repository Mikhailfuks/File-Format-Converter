#include <iostream>
#include <fstream>
#include <string>
#include <vector>

using namespace std;

// Структура для хранения информации о файле
struct FileInfo {
    string filename;
    string format;
    string data;
};

// Функция для чтения файла
FileInfo readFile(const string& filename) {
    ifstream file(filename);
    FileInfo fileInfo;
    fileInfo.filename = filename;

    if (file.is_open()) {
        string line;
        while (getline(file, line)) {
            fileInfo.data += line + "\n";
        }
        file.close();
    } else {
        cerr << "Ошибка открытия файла: " << filename << endl;
    }

    // Определение формата файла (простое предположение)
    if (filename.find(".txt") != string::npos) {
        fileInfo.format = "TXT";
    } else if (filename.find(".csv") != string::npos) {
        fileInfo.format = "CSV";
    } else {
        fileInfo.format = "Unknown";
    }

    return fileInfo;
}

// Функция для записи файла
void writeFile(const FileInfo& fileInfo, const string& newFilename) {
    ofstream file(newFilename);

    if (file.is_open()) {
        file << fileInfo.data;
        file.close();
        cout << "Файл успешно сохранен: " << newFilename << endl;
    } else {
        cerr << "Ошибка сохранения файла: " << newFilename << endl;
    }
}

// Функция для конвертации формата файла (простое предположение)
FileInfo convertFormat(const FileInfo& fileInfo, const string& newFormat) {
    FileInfo newFileInfo = fileInfo;

    if (fileInfo.format == "TXT" && newFormat == "CSV") {
        // Преобразование TXT в CSV
        // (Допустим, разделитель - запятая)
        replace(newFileInfo.data.begin(), newFileInfo.data.end(), '\n', ',');
        newFileInfo.format = "CSV";
    } else if (fileInfo.format == "CSV" && newFormat == "TXT") {
        // Преобразование CSV в TXT
        // (Допустим, разделитель - запятая)
        replace(newFileInfo.data.begin(), newFileInfo.data.end(), ',', '\n');
        newFileInfo.format = "TXT";
    }

    return newFileInfo;
}

int main() {
    string filename, newFilename, newFormat;

    cout << "Введите имя файла: ";
    cin >> filename;

    cout << "Введите имя нового файла: ";
    cin >> newFilename;

    cout << "Введите новый формат файла (TXT, CSV): ";
    cin >> newFormat;

    FileInfo fileInfo = readFile(filename);

    if (fileInfo.format != "Unknown") {
        FileInfo newFileInfo = convertFormat(fileInfo, newFormat);
        writeFile(newFileInfo, newFilename);
    } else {
        cerr << "Неизвестный формат файла!" << endl;
    }

    return 0;
}
