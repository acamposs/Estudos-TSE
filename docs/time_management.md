# Gerenciador de Estudos

## Progresso das Matérias

| Matéria                 | Horas Estudadas | Meta de Horas |
|-------------------------|-----------------|---------------|
| Português               | <span id="horas-portugues">0</span> | 30 |
| Inglês                  | <span id="horas-ingles">0</span> | 20 |
| Direito Administrativo  | <span id="horas-direito">0</span> | 25 |
| Programação de Sistemas | <span id="horas-programacao">0</span> | 40 |
| Banco de Dados          | <span id="horas-banco">0</span> | 30 |


## Insira os Dados

<form id="studyForm">
    <label for="subject">Matéria:</label>
    <select id="subject" name="subject" required>
        <option value="Português">Português</option>
        <option value="Inglês">Inglês</option>
        <option value="Direito Administrativo">Direito Administrativo</option>
        <option value="Programação de Sistemas">Programação de Sistemas</option>
        <option value="Banco de Dados">Banco de Dados</option>
    </select>
    
    <label for="hours">Horas Estudadas:</label>
    <input type="number" id="hours" name="hours" required>
    
    <button type="submit">Adicionar ao Gráfico</button>
</form>

## Semana Atual: <span id="currentWeek"></span>

<canvas id="studyChart" width="400" height="200"></canvas>

## Horas Trabalhadas da Semana
<table id="studyTable">
    <thead>
        <tr>
            <th>Matéria</th>
            <th>Horas Estudadas</th>
        </tr>
    </thead>
    <tbody>
        <!-- Dados inseridos dinamicamente -->
    </tbody>
</table>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
// Inicializa o gráfico
var ctx = document.getElementById('studyChart').getContext('2d');
var studyChart = new Chart(ctx, {
    type: 'bar',
    data: {
        labels: ['Português', 'Inglês', 'Direito Administrativo', 'Programação de Sistemas', 'Banco de Dados'],
        datasets: [{
            label: 'Horas Estudadas',
            data: [0, 0, 0, 0, 0],
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
        },
        {
            label: 'Meta de Horas',
            data: [30, 20, 25, 40, 30],
            backgroundColor: 'rgba(153, 102, 255, 0.2)',
            borderColor: 'rgba(153, 102, 255, 1)',
            borderWidth: 1
        }]
    },
    options: {
        scales: {
            y: {
                beginAtZero: true
            }
        }
    }
});

// Função para resetar os dados do gráfico toda segunda-feira
function resetChartOnMonday() {
    var today = new Date();
    var dayOfWeek = today.getDay(); // 0 = Domingo, 1 = Segunda, etc.
    
    if (dayOfWeek === 1) { // Segunda-feira
        studyChart.data.datasets[0].data = [0, 0, 0, 0, 0]; // Resetar horas estudadas
        studyChart.update();
        
        // Limpar a tabela de horas da semana
        document.getElementById('studyTable').getElementsByTagName('tbody')[0].innerHTML = '';
    }
}

// Verifica se hoje é segunda-feira e reseta o gráfico se necessário
resetChartOnMonday();

// Atualiza a tabela de horas estudadas e o gráfico
document.getElementById('studyForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    var subject = document.getElementById('subject').value;
    var hours = parseInt(document.getElementById('hours').value);
    
    // Atualiza as horas estudadas no gráfico
    var subjectIndex = studyChart.data.labels.indexOf(subject);
    studyChart.data.datasets[0].data[subjectIndex] += hours;
    studyChart.update();
    
    // Atualiza a tabela de progresso das matérias
    document.getElementById('horas-' + subject.toLowerCase().replace(' ', '-')).innerText = studyChart.data.datasets[0].data[subjectIndex];
    
    // Atualiza a tabela de horas da semana
    var tableBody = document.getElementById('studyTable').getElementsByTagName('tbody')[0];
    var newRow = tableBody.insertRow();
    var cell1 = newRow.insertCell(0);
    var cell2 = newRow.insertCell(1);
    cell1.innerHTML = subject;
    cell2.innerHTML = hours;
    
    // Limpa o formulário
    document.getElementById('studyForm').reset();
});

// Função para calcular a semana atual e exibi-la
function getCurrentWeek() {
    var today = new Date();
    var firstDayOfYear = new Date(today.getFullYear(), 0, 1);
    var pastDaysOfYear = (today - firstDayOfYear) / 86400000;
    var weekNumber = Math.ceil((pastDaysOfYear + firstDayOfYear.getDay() + 1) / 7);
    return weekNumber;
}

// Exibe a semana atual
document.getElementById('currentWeek').innerText = "Semana " + getCurrentWeek();
</script>
