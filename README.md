<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calcolatore Ecocardiografico</title>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --success: #27ae60;
            --warning: #e67e22;
            --danger: #e74c3c;
            --light: #ecf0f1;
            --dark: #34495e;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: var(--dark);
            line-height: 1.6;
            padding: 20px;
        }
        
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            padding: 30px;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid var(--light);
        }
        
        h1 {
            color: var(--primary);
            margin-bottom: 10px;
        }
        
        .description {
            color: #7f8c8d;
            font-size: 1.1rem;
        }
        
        .calculator-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin-bottom: 30px;
        }
        
        .calculator-card {
            background-color: white;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            border-left: 4px solid var(--secondary);
        }
        
        .calculator-card h2 {
            color: var(--primary);
            margin-bottom: 15px;
            font-size: 1.3rem;
        }
        
        .input-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: var(--dark);
        }
        
        input, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
        }
        
        button {
            background-color: var(--secondary);
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: background-color 0.3s;
            width: 100%;
        }
        
        button:hover {
            background-color: #2980b9;
        }
        
        .result {
            margin-top: 15px;
            padding: 15px;
            border-radius: 4px;
            font-weight: 600;
            text-align: center;
            display: none;
        }
        
        .normal {
            background-color: rgba(39, 174, 96, 0.2);
            color: var(--success);
            border: 1px solid var(--success);
        }
        
        .anomalo {
            background-color: rgba(231, 76, 60, 0.2);
            color: var(--danger);
            border: 1px solid var(--danger);
        }
        
        .reference-values {
            background-color: var(--light);
            padding: 20px;
            border-radius: 8px;
            margin-top: 30px;
        }
        
        .reference-values h2 {
            color: var(--primary);
            margin-bottom: 15px;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: var(--primary);
            color: white;
        }
        
        tr:nth-child(even) {
            background-color: #f8f9fa;
        }
        
        @media (max-width: 768px) {
            .calculator-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Calcolatore Ecocardiografico</h1>
            <p class="description">Calcola i parametri cardiaci e confrontali con i valori di normalità</p>
        </header>
        
        <div class="calculator-grid">
            <!-- Calcolatore Massa Ventricolare -->
            <div class="calculator-card">
                <h2>Massa Ventricolare Sinistra</h2>
                <div class="input-group">
                    <label for="peso">Peso corporeo (kg)</label>
                    <input type="number" id="peso" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="altezza">Altezza (cm)</label>
                    <input type="number" id="altezza" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="ivs">Spessore setto interventricolare (mm)</label>
                    <input type="number" id="ivs" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="dvd">Diametro diastolico ventricolo sinistro (mm)</label>
                    <input type="number" id="dvd" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="pwd">Spessore parete posteriore (mm)</label>
                    <input type="number" id="pwd" min="0" step="0.1">
                </div>
                <button onclick="calcolaMassaVentricolare()">Calcola Massa Ventricolare</button>
                <div id="risultatoMassa" class="result"></div>
            </div>
            
            <!-- Calcolatore Spessore Relativo -->
            <div class="calculator-card">
                <h2>Spessore Relativo Parete</h2>
                <div class="input-group">
                    <label for="ivsRWT">Spessore setto interventricolare (mm)</label>
                    <input type="number" id="ivsRWT" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="pwdRWT">Spessore parete posteriore (mm)</label>
                    <input type="number" id="pwdRWT" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="dvdRWT">Diametro diastolico ventricolo sinistro (mm)</label>
                    <input type="number" id="dvdRWT" min="0" step="0.1">
                </div>
                <button onclick="calcolaSpessoreRelativo()">Calcola Spessore Relativo</button>
                <div id="risultatoRWT" class="result"></div>
            </div>
            
            <!-- Calcolatore Volume Telediastolico -->
            <div class="calculator-card">
                <h2>Volume Telediastolico Ventricolo Sinistro</h2>
                <div class="input-group">
                    <label for="lunghezza">Lunghezza ventricolo (cm)</label>
                    <input type="number" id="lunghezza" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="area1">Area 1 (cm²)</label>
                    <input type="number" id="area1" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="area2">Area 2 (cm²)</label>
                    <input type="number" id="area2" min="0" step="0.1">
                </div>
                <button onclick="calcolaVolumeTelediastolico()">Calcola Volume Telediastolico</button>
                <div id="risultatoEDV" class="result"></div>
            </div>
            
            <!-- Calcolatore Volume Atrio Sinistro -->
            <div class="calculator-card">
                <h2>Volume Atrio Sinistro</h2>
                <div class="input-group">
                    <label for="areaLA">Area atrio sinistro (cm²)</label>
                    <input type="number" id="areaLA" min="0" step="0.1">
                </div>
                <div class="input-group">
                    <label for="lunghezzaLA">Lunghezza atrio sinistro (cm)</label>
                    <input type="number" id="lunghezzaLA" min="0" step="0.1">
                </div>
                <button onclick="calcolaVolumeAtrio()">Calcola Volume Atrio Sinistro</button>
                <div id="risultatoLA" class="result"></div>
            </div>
        </div>
        
        <div class="reference-values">
            <h2>Valori di Riferimento</h2>
            <table>
                <thead>
                    <tr>
                        <th>Parametro</th>
                        <th>Valori Normali</th>
                        <th>Unità di Misura</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Massa Ventricolare Sinistra (Uomo)</td>
                        <td>49-115 g/m²</td>
                        <td>g/m² (indicizzata alla superficie corporea)</td>
                    </tr>
                    <tr>
                        <td>Massa Ventricolare Sinistra (Donna)</td>
                        <td>43-95 g/m²</td>
                        <td>g/m² (indicizzata alla superficie corporea)</td>
                    </tr>
                    <tr>
                        <td>Spessore Relativo Parete</td>
                        <td>&lt; 0.42</td>
                        <td>adimensionale</td>
                    </tr>
                    <tr>
                        <td>Volume Telediastolico Ventricolo Sinistro (Uomo)</td>
                        <td>62-150 ml</td>
                        <td>ml</td>
                    </tr>
                    <tr>
                        <td>Volume Telediastolico Ventricolo Sinistro (Donna)</td>
                        <td>56-104 ml</td>
                        <td>ml</td>
                    </tr>
                    <tr>
                        <td>Volume Atrio Sinistro</td>
                        <td>22-52 ml/m²</td>
                        <td>ml/m² (indicizzato alla superficie corporea)</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <script>
        // Calcola la superficie corporea (formula di Du Bois)
        function calcolaSuperficieCorporea(peso, altezza) {
            return 0.007184 * Math.pow(peso, 0.425) * Math.pow(altezza, 0.725);
        }
        
        // Calcola la massa ventricolare sinistra (formula di Devereux)
        function calcolaMassaVentricolare() {
            const ivs = parseFloat(document.getElementById('ivs').value);
            const dvd = parseFloat(document.getElementById('dvd').value);
            const pwd = parseFloat(document.getElementById('pwd').value);
            const peso = parseFloat(document.getElementById('peso').value);
            const altezza = parseFloat(document.getElementById('altezza').value);
            
            if (isNaN(ivs) || isNaN(dvd) || isNaN(pwd) || isNaN(peso) || isNaN(altezza)) {
                alert("Inserisci tutti i valori richiesti");
                return;
            }
            
            // Formula: 0.8 * [1.04 * ((IVS + LVID + PWT)^3 - LVID^3)] + 0.6
            const massa = 0.8 * (1.04 * (Math.pow(ivs + dvd + pwd, 3) - Math.pow(dvd, 3))) + 0.6;
            
            // Calcola superficie corporea
            const superficie = calcolaSuperficieCorporea(peso, altezza);
            
            // Massa indicizzata alla superficie corporea
            const massaIndicizzata = massa / superficie;
            
            const risultato = document.getElementById('risultatoMassa');
            risultato.style.display = 'block';
            
            // Valori di riferimento: Uomini 49-115 g/m², Donne 43-95 g/m²
            if (massaIndicizzata >= 43 && massaIndicizzata <= 115) {
                risultato.className = 'result normal';
                risultato.innerHTML = `Massa ventricolare: ${massa.toFixed(2)} g<br>
                                      Massa indicizzata: ${massaIndicizzata.toFixed(2)} g/m²<br>
                                      <strong>VALORE NORMALE</strong>`;
            } else {
                risultato.className = 'result anomalo';
                risultato.innerHTML = `Massa ventricolare: ${massa.toFixed(2)} g<br>
                                      Massa indicizzata: ${massaIndicizzata.toFixed(2)} g/m²<br>
                                      <strong>VALORE ANOMALO</strong>`;
            }
        }
        
        // Calcola lo spessore relativo della parete (RWT)
        function calcolaSpessoreRelativo() {
            const ivs = parseFloat(document.getElementById('ivsRWT').value);
            const pwd = parseFloat(document.getElementById('pwdRWT').value);
            const dvd = parseFloat(document.getElementById('dvdRWT').value);
            
            if (isNaN(ivs) || isNaN(pwd) || isNaN(dvd)) {
                alert("Inserisci tutti i valori richiesti");
                return;
            }
            
            // RWT = (2 * spessore parete posteriore) / diametro diastolico ventricolo sinistro
            const rwt = (2 * pwd) / dvd;
            
            const risultato = document.getElementById('risultatoRWT');
            risultato.style.display = 'block';
            
            // Valore di riferimento: < 0.42
            if (rwt < 0.42) {
                risultato.className = 'result normal';
                risultato.innerHTML = `Spessore relativo parete: ${rwt.toFixed(2)}<br>
                                      <strong>VALORE NORMALE</strong>`;
            } else {
                risultato.className = 'result anomalo';
                risultato.innerHTML = `Spessore relativo parete: ${rwt.toFixed(2)}<br>
                                      <strong>VALORE ANOMALO (ipertrofia concentrica)</strong>`;
            }
        }
        
        // Calcola il volume telediastolico del ventricolo sinistro (metodo biplanare)
        function calcolaVolumeTelediastolico() {
            const lunghezza = parseFloat(document.getElementById('lunghezza').value);
            const area1 = parseFloat(document.getElementById('area1').value);
            const area2 = parseFloat(document.getElementById('area2').value);
            
            if (isNaN(lunghezza) || isNaN(area1) || isNaN(area2)) {
                alert("Inserisci tutti i valori richiesti");
                return;
            }
            
            // Volume = (8 * A1 * A2) / (3 * π * L)
            const volume = (8 * area1 * area2) / (3 * Math.PI * lunghezza);
            
            const risultato = document.getElementById('risultatoEDV');
            risultato.style.display = 'block';
            
            // Valori di riferimento: Uomini 62-150 ml, Donne 56-104 ml
            if (volume >= 56 && volume <= 150) {
                risultato.className = 'result normal';
                risultato.innerHTML = `Volume telediastolico: ${volume.toFixed(2)} ml<br>
                                      <strong>VALORE NORMALE</strong>`;
            } else {
                risultato.className = 'result anomalo';
                risultato.innerHTML = `Volume telediastolico: ${volume.toFixed(2)} ml<br>
                                      <strong>VALORE ANOMALO</strong>`;
            }
        }
        
        // Calcola il volume dell'atrio sinistro (metodo area-lunghezza)
        function calcolaVolumeAtrio() {
            const area = parseFloat(document.getElementById('areaLA').value);
            const lunghezza = parseFloat(document.getElementById('lunghezzaLA').value);
            const peso = parseFloat(document.getElementById('peso').value);
            const altezza = parseFloat(document.getElementById('altezza').value);
            
            if (isNaN(area) || isNaN(lunghezza) || isNaN(peso) || isNaN(altezza)) {
                alert("Inserisci tutti i valori richiesti (inclusi peso e altezza per il calcolo della superficie corporea)");
                return;
            }
            
            // Volume = (8 * A²) / (3 * π * L)
            const volume = (8 * Math.pow(area, 2)) / (3 * Math.PI * lunghezza);
            
            // Calcola superficie corporea
            const superficie = calcolaSuperficieCorporea(peso, altezza);
            
            // Volume indicizzato alla superficie corporea
            const volumeIndicizzato = volume / superficie;
            
            const risultato = document.getElementById('risultatoLA');
            risultato.style.display = 'block';
            
            // Valore di riferimento: 22-52 ml/m²
            if (volumeIndicizzato >= 22 && volumeIndicizzato <= 52) {
                risultato.className = 'result normal';
                risultato.innerHTML = `Volume atrio sinistro: ${volume.toFixed(2)} ml<br>
                                      Volume indicizzato: ${volumeIndicizzato.toFixed(2)} ml/m²<br>
                                      <strong>VALORE NORMALE</strong>`;
            } else {
                risultato.className = 'result anomalo';
                risultato.innerHTML = `Volume atrio sinistro: ${volume.toFixed(2)} ml<br>
                                      Volume indicizzato: ${volumeIndicizzato.toFixed(2)} ml/m²<br>
                                      <strong>VALORE ANOMALO</strong>`;
            }
        }
    </script>
</body>
</html>
