<?php
echo "<h1>🔧 Диагностика системы</h1>";
echo "<pre>";

echo "=== Диагностика окружения ===\n\n";

echo "1. Проверка PHP:\n";
echo "Версия PHP: " . PHP_VERSION . "\n";
echo "Расширение PDO: " . (extension_loaded('pdo') ? '✅' : '❌') . "\n";
echo "Расширение PDO MySQL: " . (extension_loaded('pdo_mysql') ? '✅' : '❌') . "\n\n";

echo "2. Проверка сети:\n";
echo "Пинг БД: ";
$host = 'db';
$port = 3306;
$timeout = 5;

$socket = @fsockopen($host, $port, $errno, $errstr, $timeout);
if ($socket) {
    echo "✅ Хост $host доступен на порту $port\n";
    fclose($socket);
} else {
    echo "❌ Не удается подключиться к $host:$port - $errstr\n";
}

echo "\n3. Проверка файлов:\n";
$files = ['database.php', 'db_operations.php', 'db_info.php', 'test_php.php'];
foreach ($files as $file) {
    $exists = file_exists($file);
    echo "$file: " . ($exists ? '✅ Существует' : '❌ Отсутствует') . "\n";
    if ($exists) {
        echo "   Размер: " . filesize($file) . " байт\n";
        echo "   Права: " . substr(sprintf('%o', fileperms($file)), -4) . "\n";
    }
}

echo "\n4. Переменные окружения:\n";
echo "DB_HOST: " . ($_ENV['DB_HOST'] ?? 'Не установлено') . "\n";
echo "DB_DATABASE: " . ($_ENV['DB_DATABASE'] ?? 'Не установлено') . "\n";
echo "DB_USERNAME: " . ($_ENV['DB_USERNAME'] ?? 'Не установлено') . "\n";

echo "</pre>";
?>
