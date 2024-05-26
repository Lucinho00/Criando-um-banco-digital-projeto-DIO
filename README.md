# Criando-um-banco-digital-projeto-DIO
Primeiro, foi criado uma classe Conta como classe base, que será uma classe abstrata contendo os métodos básicos que todas as contas devem ter:
public abstract class Conta {
    private int numero;
    protected double saldo;

    public Conta(int numero) {
        this.numero = numero;
        this.saldo = 0.0;
    }

    public int getNumero() {
        return numero;
    }

    public double getSaldo() {
        return saldo;
    }

    public void depositar(double valor) {
        saldo += valor;
        System.out.println("Depósito de " + valor + " realizado. Novo saldo: " + saldo);
    }

    public abstract void sacar(double valor);

    public abstract void transferir(Conta destino, double valor);
}
Depois criei duas classes filhas, ContaCorrente e ContaPoupanca, que herdarão de Conta e implementarão os métodos abstratos sacar e transferir de acordo com suas regras específicas:
public class ContaCorrente extends Conta {
    private double limiteChequeEspecial;

    public ContaCorrente(int numero, double limiteChequeEspecial) {
        super(numero);
        this.limiteChequeEspecial = limiteChequeEspecial;
    }

    @Override
    public void sacar(double valor) {
        if (saldo + limiteChequeEspecial >= valor) {
            saldo -= valor;
            System.out.println("Saque de " + valor + " realizado. Novo saldo: " + saldo);
        } else {
            System.out.println("Saldo insuficiente.");
        }
    }

    @Override
    public void transferir(Conta destino, double valor) {
        if (saldo >= valor) {
            saldo -= valor;
            destino.depositar(valor);
            System.out.println("Transferência de " + valor + " realizada para conta " + destino.getNumero() + ".");
        } else {
            System.out.println("Saldo insuficiente.");
        }
    }
}

public class ContaPoupanca extends Conta {
    private double taxaRendimento;

    public ContaPoupanca(int numero, double taxaRendimento) {
        super(numero);
        this.taxaRendimento = taxaRendimento;
    }

    @Override
    public void sacar(double valor) {
        if (saldo >= valor) {
            saldo -= valor;
            System.out.println("Saque de " + valor + " realizado. Novo saldo: " + saldo);
        } else {
            System.out.println("Saldo insuficiente.");
        }
    }

    @Override
    public void transferir(Conta destino, double valor) {
        if (saldo >= valor) {
            saldo -= valor;
            destino.depositar(valor);
            System.out.println("Transferência de " + valor + " realizada para conta " + destino.getNumero() + ".");
        } else {
            System.out.println("Saldo insuficiente.");
        }
    }

    public void calcularRendimento() {
        double rendimento = saldo * taxaRendimento / 100;
        saldo += rendimento;
        System.out.println("Rendimento aplicado. Novo saldo: " + saldo);
    }
}
 por fim criei um teste simples para verificar se a implementação está funcionando corretamente:
 public class Main {
    public static void main(String[] args) {
        ContaCorrente contaCorrente = new ContaCorrente(123, 1000.0);
        ContaPoupanca contaPoupanca = new ContaPoupanca(456, 1.5);

        contaCorrente.depositar(500.0);
        contaCorrente.sacar(200.0);

        contaPoupanca.depositar(1000.0);
        contaPoupanca.sacar(200.0);

        contaCorrente.transferir(contaPoupanca, 300.0);
        contaPoupanca.calcularRendimento();
    }
}
