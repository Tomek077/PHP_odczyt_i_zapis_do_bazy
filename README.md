**Temat lekcji: Tworzenie prostej bazy danych w MySQL oraz odczyt i zapis danych z wykorzystaniem PHP (podejście proceduralne)**

---

**Cel lekcji:**

- Nauczysz się tworzyć prostą bazę danych w MySQL.
- Nauczysz się zapisywać dane do bazy i odczytywać je z wykorzystaniem PHP.

---

**Wprowadzenie:**

Dzisiaj stworzymy prostą bazę danych o nazwie "szkola", w której będziemy przechowywać informacje o uczniach. Następnie napiszemy skrypty PHP, które pozwolą na dodawanie nowych uczniów oraz wyświetlanie listy uczniów na stronie internetowej.

---

### **1. Tworzenie bazy danych w MySQL**

**a) Uruchomienie phpMyAdmin lub innego narzędzia do zarządzania MySQL**

- Otwórz przeglądarkę internetową i przejdź do phpMyAdmin (np. `http://localhost/phpmyadmin`).

**b) Tworzenie nowej bazy danych**

- Kliknij na zakładkę "Bazy danych".
- W polu "Utwórz bazę danych" wpisz nazwę `szkola` i kliknij "Utwórz".

**c) Tworzenie tabeli "uczniowie"**

- Kliknij na nowo utworzoną bazę danych "szkola".
- W polu "Utwórz tabelę" wpisz nazwę `uczniowie` i ustaw liczbę kolumn na `4`. Kliknij "Wykonaj".

**d) Definiowanie struktury tabeli**

Uzupełnij pola zgodnie z poniższą tabelą:

| Kolumna  | Typ         | Długość/Wartości | Atrybuty | Indeks     | Auto_increment |
|----------|-------------|------------------|----------|------------|----------------|
| id       | INT         |                  |          | PRIMARY    | TAK            |
| imie     | VARCHAR     | 50               |          |            |                |
| nazwisko | VARCHAR     | 50               |          |            |                |
| klasa    | VARCHAR     | 10               |          |            |                |

- Po uzupełnieniu wszystkich pól kliknij "Zapisz".

---

### **2. Tworzenie formularza HTML do wprowadzania danych**

**a) Utworzenie pliku "formularz.html"**

- Otwórz edytor tekstu (np. Notepad++).
- Wklej poniższy kod:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Formularz ucznia</title>
</head>
<body>
    <h1>Dodaj nowego ucznia</h1>
    <form action="dodaj_ucznia.php" method="post">
        Imię:<br>
        <input type="text" name="imie"><br><br>
        Nazwisko:<br>
        <input type="text" name="nazwisko"><br><br>
        Klasa:<br>
        <input type="text" name="klasa"><br><br>
        <input type="submit" value="Dodaj ucznia">
    </form>
</body>
</html>
```

- Zapisz plik jako `formularz.html` w folderze serwera WWW (np. `htdocs` w przypadku XAMPP).

---

### **3. Tworzenie skryptu PHP do zapisywania danych**

**a) Utworzenie pliku "dodaj_ucznia.php"**

- W edytorze tekstu utwórz nowy plik i wklej poniższy kod:

```php
<?php
// Połączenie z bazą danych
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "szkola";

$conn = mysqli_connect($servername, $username, $password, $dbname);

// Sprawdzenie połączenia
if (!$conn) {
    die("Błąd połączenia: " . mysqli_connect_error());
}

// Pobranie i zabezpieczenie danych z formularza
$imie = mysqli_real_escape_string($conn, $_POST['imie']);
$nazwisko = mysqli_real_escape_string($conn, $_POST['nazwisko']);
$klasa = mysqli_real_escape_string($conn, $_POST['klasa']);

// Wstawienie danych do bazy
$sql = "INSERT INTO uczniowie (imie, nazwisko, klasa) VALUES ('$imie', '$nazwisko', '$klasa')";

if (mysqli_query($conn, $sql)) {
    echo "Nowy uczeń został dodany pomyślnie.<br>";
    echo "<a href='wyswietl_uczniow.php'>Wyświetl listę uczniów</a>";
} else {
    echo "Błąd: " . $sql . "<br>" . mysqli_error($conn);
}

// Zamknięcie połączenia
mysqli_close($conn);
?>
```

- Zapisz plik jako `dodaj_ucznia.php` w folderze serwera WWW.

---

### **4. Tworzenie skryptu PHP do wyświetlania danych**

**a) Utworzenie pliku "wyswietl_uczniow.php"**

- W edytorze tekstu utwórz nowy plik i wklej poniższy kod:

```php
<?php
// Połączenie z bazą danych
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "szkola";

$conn = mysqli_connect($servername, $username, $password, $dbname);

// Sprawdzenie połączenia
if (!$conn) {
    die("Błąd połączenia: " . mysqli_connect_error());
}

// Pobranie danych z bazy
$sql = "SELECT * FROM uczniowie";
$result = mysqli_query($conn, $sql);

// Wyświetlanie danych
if (mysqli_num_rows($result) > 0) {
    echo "<h1>Lista uczniów</h1>";
    echo "<table border='1'>
            <tr>
                <th>ID</th>
                <th>Imię</th>
                <th>Nazwisko</th>
                <th>Klasa</th>
            </tr>";
    while($row = mysqli_fetch_assoc($result)) {
        echo "<tr>
                <td>".$row['id']."</td>
                <td>".$row['imie']."</td>
                <td>".$row['nazwisko']."</td>
                <td>".$row['klasa']."</td>
              </tr>";
    }
    echo "</table>";
} else {
    echo "Brak danych.";
}

// Zamknięcie połączenia
mysqli_close($conn);
?>
```

- Zapisz plik jako `wyswietl_uczniow.php` w folderze serwera WWW.

---

### **5. Testowanie aplikacji**

**a) Uruchomienie serwera WWW**

- Upewnij się, że serwer Apache jest uruchomiony (np. w XAMPP).

**b) Dodawanie nowego ucznia**

- Otwórz przeglądarkę internetową.
- Przejdź do adresu `http://localhost/formularz.html`.
- Wypełnij formularz danymi ucznia i kliknij "Dodaj ucznia".

**c) Wyświetlanie listy uczniów**

- Po dodaniu ucznia pojawi się komunikat oraz link do wyświetlenia listy uczniów.
- Kliknij na link "Wyświetl listę uczniów", aby zobaczyć wprowadzone dane.

---

### **Podsumowanie:**

- Stworzyliśmy bazę danych i tabelę w MySQL.
- Napisaliśmy formularz HTML do wprowadzania danych.
- Utworzyliśmy skrypty PHP do zapisywania i wyświetlania danych.
- Przetestowaliśmy działanie aplikacji.

---

### **Zadania do samodzielnego wykonania:**

1. **Dodaj nowe pole "wiek" (INT) do tabeli "uczniowie". Zaktualizuj formularz oraz skrypty PHP tak, aby umożliwić wprowadzanie i wyświetlanie wieku ucznia.**

2. **Stwórz skrypt "edytuj_ucznia.php", który pozwoli na edycję danych istniejącego ucznia. Dodaj odpowiednie linki lub przyciski w tabeli wyświetlającej listę uczniów.**

3. **Zmień wygląd tabeli w skrypcie "wyswietl_uczniow.php", dodając style CSS, aby była bardziej czytelna i estetyczna.**

---

**Dodatkowe uwagi:**

- **Bezpieczeństwo danych:** W naszych skryptach użyliśmy funkcji `mysqli_real_escape_string`, aby zabezpieczyć dane wprowadzane przez użytkownika przed potencjalnymi atakami (np. SQL Injection).
- **Komentarze w kodzie:** Zachęcamy do dodawania komentarzy w kodzie, aby lepiej zrozumieć działanie poszczególnych fragmentów.

---

**Powodzenia w dalszej nauce!**
