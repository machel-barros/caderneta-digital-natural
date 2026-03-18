#include <iostream>
#include <fstream>
#include <iomanip>
using namespace std;

const int MAX_ALUNOS = 10;

int carregarNomes(string nome[]) {
    ifstream ficheiro("nome.txt");
    int total = 0;

    if (!ficheiro) {
        cout << "Erro ao abrir ficheiro de nomes!" << endl;
        return 0;
    }

    while (getline(ficheiro, nome[total]) && total < MAX_ALUNOS) {
        total++;
    }

    ficheiro.close();
    return total;
}

void inserirNotas(float t1[], float t2[], int total, string nome[]) {
    for (int i = 0; i < total; i++) {
        cout << "Aluno: " << nome[i] << endl;

        cout << "Nota Teste 1: ";
        cin >> t1[i];

        cout << "Nota Teste 2: ";
        cin >> t2[i];

        cout << endl;
    }
}

void calcularMedias(float t1[], float t2[], float medias[], int total) {
    for (int i = 0; i < total; i++) {
        medias[i] = (t1[i] + t2[i]) / 2;
    }
}

void guardarPauta(string nome[], float t1[], float t2[], float medias[], int total) {
    ofstream ficheiro("pauta_final.txt");

    if (!ficheiro) {
        cout << "Erro ao criar ficheiro!" << endl;
        return;
    }

    ficheiro << left << setw(20) << "Nome"
             << setw(10) << "Teste1"
             << setw(10) << "Teste2"
             << setw(10) << "Media"
             << setw(12) << "Estado" << endl;

    ficheiro << "-------------------------------------------------------------" << endl;

    for (int i = 0; i < total; i++) {
        string estado = (medias[i] >= 10) ? "Aprovado" : "Reprovado";

        ficheiro << left << setw(20) << nome [i]
                 << setw(10) << fixed << setprecision(2) << t1[i]
                 << setw(10) << t2[i]
                 << setw(10) << medias[i]
                 << setw(12) << estado << endl;
    }

    ficheiro.close();
}

int main() {
    string nome[MAX_ALUNOS];
    float notas1[MAX_ALUNOS], notas2[MAX_ALUNOS], medias[MAX_ALUNOS];

    int total = carregarNomes(nome);

    if (total == 0) {
        cout << "Nenhum aluno carregado!" << endl;
        return 1;
    }

    cout << "Total de alunos: " << total << endl << endl;

    inserirNotas(notas1, notas2, total, nome);
    calcularMedias(notas1, notas2, medias, total);
    guardarPauta(nome, notas1, notas2, medias, total);

    cout << "Pauta final guardada com sucesso!" << endl;

    return 0;
}