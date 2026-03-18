#include <iostream>
#include <fstream>
#include <iomanip>
using namespace std;

const int MAX = 30;

// Função para carregar nomes do ficheiro
int carregarNomes(string nomes[]) {
    ifstream ficheiro("nomes.txt");
    int i = 0;

    if (!ficheiro) {
        cout << "Erro ao abrir o ficheiro de nomes!\n";
        return 0;
    }

    while (getline(ficheiro, nomes[i]) && i < MAX) {
        i++;
    }

    ficheiro.close();
    return i; // retorna o número de alunos carregados
}

int main() {
    string nomes[MAX];
    float notas_teste1[MAX], notas_teste2[MAX], media[MAX];
    int totalAlunos;

    // Carregar nomes do ficheiro
    totalAlunos = carregarNomes(nomes);

    if (totalAlunos == 0) {
        cout << "Nenhum aluno encontrado.\n";
        return 1;
    }

    // Inserir notas
    cout << "=== Inserção de Notas ===\n";
    for (int i = 0; i < totalAlunos; i++) {
        cout << "\nAluno: " << nomes[i] << endl;

        cout << "Nota Teste 1: ";
        cin >> notas_teste1[i];

        cout << "Nota Teste 2: ";
        cin >> notas_teste2[i];

        // Calcula média
        media[i] = (notas_teste1[i] + notas_teste2[i]) / 2;
    }

    // Criar ficheiro de saída
    ofstream pauta("pauta_final.txt");

    if (!pauta) {
        cout << "Erro ao criar o ficheiro!\n";
        return 1;
    }

    // Cabeçalho da tabela
    pauta << left << setw(20) << "Nome"
          << setw(10) << "Teste1"
          << setw(10) << "Teste2"
          << setw(10) << "Media"
          << setw(12) << "Estado" << endl;

    pauta << "-----------------------------------------------------------\n";

    // Escrever dados no ficheiro
    for (int i = 0; i < totalAlunos; i++) {
        string estado = (media[i] >= 10) ? "Aprovado" : "Reprovado";

        pauta << left << setw(20) << nomes[i]
              << setw(10) << notas_teste1[i]
              << setw(10) << notas_teste2[i]
              << setw(10) << fixed << setprecision(1) << media[i]
              << setw(12) << estado << endl;
    }

    pauta.close();

    cout << "\nPauta final gerada com sucesso em 'pauta_final.txt'!\n";

    return 0;
}
