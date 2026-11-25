#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

const int MAX_CONTAS = 100;
const int MAX_MOVIMENTACOES = 100;

int localizarConta(int numero, int numeros[], int quantidade)
{
    int i = 0;

    while (i < quantidade)
    {
        if (numeros[i] == numero)
        {
            return i;
        }
        else
        {
            i++;
        }
    }

    return -1;
}

void registrarMovimentacao(
    string historico[][MAX_MOVIMENTACOES],
    int quantidadeMov[],
    int conta,
    string descricao)
{
    if (quantidadeMov[conta] < MAX_MOVIMENTACOES)
    {
        historico[conta][quantidadeMov[conta]] = descricao;
        quantidadeMov[conta]++;
    }
    else
    {
        cout << "Historico cheio!" << endl;
    }
}

int main()
{
    int numeroConta[MAX_CONTAS];
    string nomeCliente[MAX_CONTAS];
    string cpf[MAX_CONTAS];

    double saldo[MAX_CONTAS];

    string historico[MAX_CONTAS][MAX_MOVIMENTACOES];
    int quantidadeMov[MAX_CONTAS];

    int quantidadeContas = 0;
    int opcao;

    int i = 0;
    while (i < MAX_CONTAS)
    {
        saldo[i] = 0;
        quantidadeMov[i] = 0;
        i++;
    }

    do
    {
        cout << endl;
        cout << "========== BANCO INF101 ==========" << endl;
        cout << "1 - Cadastrar contas" << endl;
        cout << "2 - Listar contas cadastradas" << endl;
        cout << "3 - Consultar conta pelo numero" << endl;
        cout << "4 - Realizar depositos" << endl;
        cout << "5 - Realizar saques" << endl;
        cout << "6 - Realizar transferencias" << endl;
        cout << "7 - Exibir extrato" << endl;
        cout << "8 - Sair" << endl;
        cout << "Escolha uma opcao: ";
        cin >> opcao;

        // CADASTRAR CONTA
        if (opcao == 1)
        {
            if (quantidadeContas >= MAX_CONTAS)
            {
                cout << "Limite de contas atingido!" << endl;
            }
            else
            {
                int numero;

                cout << "Numero da conta: ";
                cin >> numero;

                int posicao = localizarConta(
                    numero,
                    numeroConta,
                    quantidadeContas
                );

                if (numero <= 0)
                {
                    cout << "Numero invalido!" << endl;
                }
                else if (posicao != -1)
                {
                    cout << "Essa conta ja esta cadastrada!" << endl;
                }
                else
                {
                    numeroConta[quantidadeContas] = numero;

                    cin.ignore();

                    cout << "Nome do cliente: ";
                    getline(cin, nomeCliente[quantidadeContas]);

                    cout << "CPF: ";
                    getline(cin, cpf[quantidadeContas]);

                    saldo[quantidadeContas] = 0;

                    registrarMovimentacao(
                        historico,
                        quantidadeMov,
                        quantidadeContas,
                        "Conta cadastrada com saldo inicial de R$ 0,00"
                    );

                    quantidadeContas++;

                    cout << "Conta cadastrada com sucesso!" << endl;
                }
            }
        }

        // LISTAR CONTAS
        else if (opcao == 2)
        {
            if (quantidadeContas == 0)
            {
                cout << "Nenhuma conta cadastrada." << endl;
            }
            else
            {
                i = 0;

                while (i < quantidadeContas)
                {
                    cout << endl;
                    cout << "Numero: " << numeroConta[i] << endl;
                    cout << "Nome: " << nomeCliente[i] << endl;
                    cout << "CPF: " << cpf[i] << endl;
                    cout << "Saldo: R$ "
                         << fixed << setprecision(2)
                         << saldo[i] << endl;
                    cout << "-------------------------" << endl;

                    i++;
                }
            }
        }

        // CONSULTAR CONTA
        else if (opcao == 3)
        {
            int numero;

            cout << "Digite o numero da conta: ";
            cin >> numero;

            int posicao = localizarConta(
                numero,
                numeroConta,
                quantidadeContas
            );

            if (posicao == -1)
            {
                cout << "Conta nao encontrada!" << endl;
            }
            else
            {
                cout << endl;
                cout << "Nome: " << nomeCliente[posicao] << endl;
                cout << "CPF: " << cpf[posicao] << endl;
                cout << "Numero: " << numeroConta[posicao] << endl;
                cout << "Saldo: R$ "
                     << fixed << setprecision(2)
                     << saldo[posicao] << endl;
            }
        }

        // DEPOSITO
        else if (opcao == 4)
        {
            int numero;
            double valor;

            cout << "Numero da conta: ";
            cin >> numero;

            int posicao = localizarConta(
                numero,
                numeroConta,
                quantidadeContas
            );

            if (posicao == -1)
            {
                cout << "Conta nao encontrada!" << endl;
            }
            else
            {
                cout << "Valor do deposito: R$ ";
                cin >> valor;

                if (valor <= 0)
                {
                    cout << "Valor invalido!" << endl;
                }
                else if (quantidadeMov[posicao] >= MAX_MOVIMENTACOES)
                {
                    cout << "Historico cheio!" << endl;
                }
                else
                {
                    saldo[posicao] = saldo[posicao] + valor;

                    registrarMovimentacao(
                        historico,
                        quantidadeMov,
                        posicao,
                        "Deposito realizado"
                    );

                    cout << "Deposito realizado!" << endl;
                    cout << "Novo saldo: R$ "
                         << fixed << setprecision(2)
                         << saldo[posicao] << endl;
                }
            }
        }

        // SAQUE
        else if (opcao == 5)
        {
            int numero;
            double valor;

            cout << "Numero da conta: ";
            cin >> numero;

            int posicao = localizarConta(
                numero,
                numeroConta,
                quantidadeContas
            );

            if (posicao == -1)
            {
                cout << "Conta nao encontrada!" << endl;
            }
            else
            {
                cout << "Valor do saque: R$ ";
                cin >> valor;

                if (valor <= 0)
                {
                    cout << "Valor invalido!" << endl;
                }
                else if (saldo[posicao] < valor)
                {
                    cout << "Saldo insuficiente!" << endl;
                }
                else if (quantidadeMov[posicao] >= MAX_MOVIMENTACOES)
                {
                    cout << "Historico cheio!" << endl;
                }
                else
                {
                    saldo[posicao] = saldo[posicao] - valor;

                    registrarMovimentacao(
                        historico,
                        quantidadeMov,
                        posicao,
                        "Saque realizado"
                    );

                    cout << "Saque realizado!" << endl;
                    cout << "Novo saldo: R$ "
                         << fixed << setprecision(2)
                         << saldo[posicao] << endl;
                }
            }
        }

        // TRANSFERENCIA
        else if (opcao == 6)
        {
            int origem;
            int destino;
            double valor;

            cout << "Conta de origem: ";
            cin >> origem;

            int posOrigem = localizarConta(
                origem,
                numeroConta,
                quantidadeContas
            );

            if (posOrigem == -1)
            {
                cout << "Conta de origem nao encontrada!" << endl;
            }
            else
            {
                cout << "Conta de destino: ";
                cin >> destino;

                int posDestino = localizarConta(
                    destino,
                    numeroConta,
                    quantidadeContas
                );

                if (posDestino == -1)
                {
                    cout << "Conta de destino nao encontrada!" << endl;
                }
                else if (posOrigem == posDestino)
                {
                    cout << "A conta de origem e destino nao podem ser iguais!" << endl;
                }
                else
                {
                    cout << "Valor da transferencia: R$ ";
                    cin >> valor;

                    if (valor <= 0)
                    {
                        cout << "Valor invalido!" << endl;
                    }
                    else if (saldo[posOrigem] < valor)
                    {
                        cout << "Saldo insuficiente!" << endl;
                    }
                    else if (
                        quantidadeMov[posOrigem] >= MAX_MOVIMENTACOES ||
                        quantidadeMov[posDestino] >= MAX_MOVIMENTACOES
                    )
                    {
                        cout << "Historico cheio!" << endl;
                    }
                    else
                    {
                        saldo[posOrigem] =
                            saldo[posOrigem] - valor;

                        saldo[posDestino] =
                            saldo[posDestino] + valor;

                        registrarMovimentacao(
                            historico,
                            quantidadeMov,
                            posOrigem,
                            "Transferencia enviada"
                        );

                        registrarMovimentacao(
                            historico,
                            quantidadeMov,
                            posDestino,
                            "Transferencia recebida"
                        );

                        cout << "Transferencia realizada com sucesso!"
                             << endl;
                    }
                }
            }
        }

        // EXTRATO
        else if (opcao == 7)
        {
            int numero;

            cout << "Numero da conta: ";
            cin >> numero;

            int posicao = localizarConta(
                numero,
                numeroConta,
                quantidadeContas
            );

            if (posicao == -1)
            {
                cout << "Conta nao encontrada!" << endl;
            }
            else
            {
                cout << endl;
                cout << "========== EXTRATO ==========" << endl;
                cout << "Cliente: "
                     << nomeCliente[posicao] << endl;

                cout << "Conta: "
                     << numeroConta[posicao] << endl;

                i = 0;

                while (i < quantidadeMov[posicao])
                {
                    cout << i + 1 << " - "
                         << historico[posicao][i] << endl;

                    i++;
                }

                cout << "Saldo atual: R$ "
                     << fixed << setprecision(2)
                     << saldo[posicao] << endl;

                cout << "=============================" << endl;
            }
        }

        // SAIR
        else if (opcao == 8)
        {
            cout << "Sistema encerrado!" << endl;
        }

        // OPCAO INVALIDA
        else
        {
            cout << "Opcao invalida! Digite novamente." << endl;
        }

    } while (opcao != 8);

    return 0;
}
