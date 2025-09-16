# PHP: File Handling

PHP has powerful functions for creating, reading, uploading, and editing files. This note covers the basics of file handling in PHP.

## Opening and Closing Files

To work with files, you first need to open them using `fopen()` and then close them using `fclose()`.

### `fopen(filename, mode)`

Opens a file or URL. Returns a file pointer resource on success, or `false` on error.

*   `filename`: The path to the file.
*   `mode`: Specifies the type of access you require to the stream.

Common modes:

*   `r`: Read only. Pointer at the beginning. (Default)
*   `r+`: Read/Write. Pointer at the beginning.
*   `w`: Write only. Creates new file or truncates existing. Pointer at the beginning.
*   `w+`: Read/Write. Creates new file or truncates existing. Pointer at the beginning.
*   `a`: Write only. Creates new file or appends to existing. Pointer at the end.
*   `a+`: Read/Write. Creates new file or appends to existing. Pointer at the end.
*   `x`: Write only. Creates new file. Returns `false` if file already exists.
*   `x+`: Read/Write. Creates new file. Returns `false` if file already exists.

### `fclose(file_pointer)`

Closes an open file pointer.

```php
<?php
$filename = "example.txt";

// Open for writing (creates or truncates)
$file = fopen($filename, "w");
if ($file) {
    echo "File opened successfully for writing.\n";
    fclose($file);
} else {
    echo "Error opening file for writing.\n";
}

// Open for reading
$file = fopen($filename, "r");
if ($file) {
    echo "File opened successfully for reading.\n";
    fclose($file);
} else {
    echo "Error opening file for reading.\n";
}
?>
```

## Reading Files

### `fread(file_pointer, length)`

Reads up to `length` bytes from the file pointer.

### `fgets(file_pointer)`

Reads a single line from the file pointer.

### `feof(file_pointer)`

Checks if the end-of-file (EOF) has been reached.

### `file_get_contents(filename)`

Reads entire file into a string. Simple for small files.

```php
<?php
$filename = "data.txt";
file_put_contents($filename, "Line 1\nLine 2\nLine 3"); // Create a dummy file

// Read entire file using file_get_contents()
echo "\n--- Reading with file_get_contents() ---\n";
$content = file_get_contents($filename);
echo $content;

// Read line by line using fgets()
echo "\n--- Reading line by line with fgets() ---\n";
$file = fopen($filename, "r");
if ($file) {
    while (!feof($file)) {
        echo fgets($file);
    }
    fclose($file);
}

// Read specific amount of bytes using fread()
echo "\n--- Reading with fread() ---\n";
$file = fopen($filename, "r");
if ($file) {
    $data = fread($file, 5); // Read first 5 bytes
    echo $data . "\n";
    fclose($file);
}
?>
```

## Writing to Files

### `fwrite(file_pointer, string)`

Writes `string` to the file pointer.

### `file_put_contents(filename, data, flags)`

Writes data to a file. If the file does not exist, it is created. If it exists, it is overwritten unless `FILE_APPEND` flag is used.

```php
<?php
$filename = "output.txt";

// Write to a new file (or overwrite existing)
file_put_contents($filename, "Hello, World!\n");
file_put_contents($filename, "This will overwrite the previous content.\n");
echo "Content written to $filename.\n";

// Append to an existing file
file_put_contents($filename, "This will be appended.\n", FILE_APPEND);
echo "Content appended to $filename.\n";

// Using fwrite with fopen
$file = fopen("log.txt", "a"); // Open in append mode
if ($file) {
    $timestamp = date("Y-m-d H:i:s");
    fwrite($file, "[$timestamp] User accessed page.\n");
    fclose($file);
    echo "Log entry added to log.txt.\n";
} else {
    echo "Error opening log.txt.\n";
}
?>
```

## Checking File Existence and Permissions

*   `file_exists(filename)`: Checks if a file or directory exists.
*   `is_readable(filename)`: Checks if a file is readable.
*   `is_writable(filename)`: Checks if a file is writable.

```php
<?php
$filename = "test_file.txt";
file_put_contents($filename, "test"); // Create a dummy file

if (file_exists($filename)) {
    echo "$filename exists.\n";
}

if (is_readable($filename)) {
    echo "$filename is readable.\n";
}

if (is_writable($filename)) {
    echo "$filename is writable.\n";
}

unlink($filename); // Clean up the dummy file
?>
```

## Deleting Files

### `unlink(filename)`

Deletes a file.

```php
<?php
$filename = "delete_me.txt";
file_put_contents($filename, "Content to be deleted.");

if (file_exists($filename)) {
    unlink($filename);
    echo "$filename deleted successfully.\n";
} else {
    echo "$filename does not exist.\n";
}
?>
```

## Working with Directories

*   `mkdir(path, mode, recursive)`: Creates a directory.
*   `rmdir(path)`: Removes an empty directory.
*   `is_dir(path)`: Checks if the given filename is a directory.
*   `scandir(directory)`: Lists files and directories inside the specified path.

```php
<?php
$dir = "my_new_directory";

if (!is_dir($dir)) {
    mkdir($dir, 0777, true); // Create recursively with full permissions
    echo "Directory '$dir' created.\n";
}

// Create a file inside the new directory
file_put_contents($dir . "/inside.txt", "Hello from inside!");

// List contents of the directory
$contents = scandir($dir);
print_r($contents);

// Clean up: remove file then directory
unlink($dir . "/inside.txt");
rmdir($dir);
echo "Directory '$dir' removed.\n";
?>
```

## File Uploads

Handling file uploads typically involves processing the `$_FILES` superglobal array and moving the uploaded file from its temporary location to a permanent one.

```php
<!-- HTML Form for Upload -->
<form action="upload.php" method="post" enctype="multipart/form-data">
    Select image to upload:
    <input type="file" name="fileToUpload" id="fileToUpload">
    <input type="submit" value="Upload Image" name="submit">
</form>
```

```php
<?php
// upload.php
$target_dir = "uploads/";
$target_file = $target_dir . basename($_FILES["fileToUpload"]["name"]);
$uploadOk = 1;
$imageFileType = strtolower(pathinfo($target_file, PATHINFO_EXTENSION));

// Check if image file is a actual image or fake image
if(isset($_POST["submit"])) {
    $check = getimagesize($_FILES["fileToUpload"]["tmp_name"]);
    if($check !== false) {
        echo "File is an image - " . $check["mime"] . ".";
        $uploadOk = 1;
    } else {
        echo "File is not an image.";
        $uploadOk = 0;
    }
}

// Check if file already exists
if (file_exists($target_file)) {
    echo "Sorry, file already exists.";
    $uploadOk = 0;
}

// Check file size (e.g., 500KB limit)
if ($_FILES["fileToUpload"]["size"] > 500000) {
    echo "Sorry, your file is too large.";
    $uploadOk = 0;
}

// Allow certain file formats
if($imageFileType != "jpg" && $imageFileType != "png" && $imageFileType != "jpeg"
&& $imageFileType != "gif" ) {
    echo "Sorry, only JPG, JPEG, PNG & GIF files are allowed.";
    $uploadOk = 0;
}

// Check if $uploadOk is set to 0 by an error
if ($uploadOk == 0) {
    echo "Sorry, your file was not uploaded.";
// if everything is ok, try to upload file
} else {
    if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $target_file)) {
        echo "The file ". htmlspecialchars( basename( $_FILES["fileToUpload"]["name"])) . " has been uploaded.";
    } else {
        echo "Sorry, there was an error uploading your file.";
    }
}
?>
```

File handling is a common task in web development, and PHP provides a comprehensive set of functions to manage files and directories effectively.