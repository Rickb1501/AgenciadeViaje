<?php
require 'config.php';

if (!isset($_SESSION['user_id'])) {
    // Redirige al usuario a la página de inicio de sesión si no está autenticado
    header("Location: login.php");
    exit();
}

// Extiende la vida de la sesión si hay actividad
if (isset($_SESSION['LAST_ACTIVITY']) && (time() - $_SESSION['LAST_ACTIVITY'] > 1800)) {
    // La sesión ha estado inactiva por más de 30 minutos
    session_unset(); // Destruye variables de sesión
    session_destroy(); // Destruye la sesión
    header("Location: login.php"); // Redirige al usuario al inicio de sesión
    exit();
}
$_SESSION['LAST_ACTIVITY'] = time(); // Actualiza el timestamp de la última actividad

// Cargar preferencias del usuario desde las cookies
if (isset($_COOKIE['user_preferences'])) {
    $_SESSION['preferences'] = json_decode($_COOKIE['user_preferences'], true);
}

echo "Bienvenido, " . $_SESSION['username'];
?>
<!DOCTYPE html>
<html>
<head>
    <title>Inicio</title>
    <link rel="stylesheet" type="text/css" href="styles.css">
    <script src="script.js"></script>
</head>
<body>
    <header>
        <h1>Agencia de Viajes</h1>
    </header>
    <a href="paquetes.php">Comprar paquetes de Viaje</a>
    <a href="">/////////////</a>
    <a href="Index.php">Vuelos y Hoteles</a>
    <div class="search-container">
        <input type="text" id="destination" placeholder="Destino">
        <input type="date" id="travel-date">
        <button onclick="search()">Buscar</button>
    </div>
    <div id="notifications-container">
        <!-- Las notificaciones se mostrarán aquí -->
    </div>
    <div id="results-container">
        <!-- Los resultados de la búsqueda se mostrarán aquí -->
    </div>
    <div class="form-container">
        <h2>Registro de Viaje</h2>
        <form action="procesarRegistro.php" method="post">
            <label for="nombreHotel">Nombre del Hotel:</label>
            <input type="text" id="nombreHotel" name="nombreHotel" required>

            <label for="ciudad">Ciudad:</label>
            <input type="text" id="ciudad" name="ciudad" required>

            <label for="pais">País:</label>
            <input type="text" id="pais" name="pais" required>

            <label for="fechaViaje">Fecha de Viaje:</label>
            <input type="date" id="fechaViaje" name="fechaViaje" required>

            <label for="duracionViaje">Duración del Viaje (días):</label>
            <input type="number" id="duracionViaje" name="duracionViaje" required>

            <input type="submit" value="Registrar">
        </form>
    </div>
    <script src="script.js"></script>
    <a href="paquetes.php">Comprar paquetes de Viaje</a>
</body>
</html>
