# Amigo-secreto
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Amigo Secreto</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f4;
        }
        .container {
            text-align: center;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        select, button {
            margin: 10px;
            padding: 10px;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Amigo Secreto</h1>
        <p>Selecciona quién eres:</p>
        <select id="participant">
            <option value="lucho">Lucho</option>
            <option value="pato">Pato</option>
            <option value="nelson">Nelson</option>
            <option value="nelson grande">Nelson Grande</option>
            <option value="claudia grande">Claudia Grande</option>
            <option value="leyni">Leyni</option>
            <option value="mauro">Mauro</option>
            <option value="mari">Mari</option>
        </select>
        <br>
        <button onclick="getSecretFriend()">Ver Amigo Secreto</button>
        <p id="result"></p>
    </div>

    <script>
        const restrictions = {
            "lucho": ["pato"],
            "pato": ["lucho"],
            "leyni": ["mauro"],
            "mauro": ["leyni"],
            "nelson": ["mari"],
            "mari": ["nelson"],
            "nelson grande": ["claudia grande"],
            "claudia grande": ["nelson grande"]
        };

        const participants = ["lucho", "pato", "nelson", "nelson grande", "claudia grande", "leyni", "mauro", "mari"];
        let assignedFriends = {};  // Object to store the assigned friends
        let availableFriends = [...participants];  // Initially all participants are available

        function getSecretFriend() {
            const selectedParticipant = document.getElementById('participant').value;

            // Check if this participant already has a friend assigned
            if (assignedFriends[selectedParticipant]) {
                document.getElementById('result').innerText = `Tu amigo secreto ya fue asignado: ${assignedFriends[selectedParticipant].charAt(0).toUpperCase() + assignedFriends[selectedParticipant].slice(1)}`;
                return;
            }

            // Filter the available friends based on restrictions and who is left
            let validFriends = availableFriends.filter(p => 
                p !== selectedParticipant && 
                !restrictions[selectedParticipant]?.includes(p) && 
                !Object.values(assignedFriends).includes(p)
            );

            if (validFriends.length === 0) {
                document.getElementById('result').innerText = "No hay amigos secretos disponibles con las restricciones dadas.";
                return;
            }

            let secretFriend = validFriends[Math.floor(Math.random() * validFriends.length)];
            assignedFriends[selectedParticipant] = secretFriend;

            // Remove the assigned friend from the available list
            availableFriends = availableFriends.filter(p => p !== secretFriend);

            document.getElementById('result').innerText = `Tu amigo secreto es: ${secretFriend.charAt(0).toUpperCase() + secretFriend.slice(1)}`;
        }
    </script>
</body>
</html>
