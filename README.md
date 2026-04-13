# Estudos001 

// Rota.java
public class Rota {
    private String origem;
    private String destino;
    private double distanciaTotal;
    private double tempoEstimado;

    public Rota(String origem, String destino, double distanciaTotal, double tempoEstimado) {
        this.origem = origem;
        this.destino = destino;
        this.distanciaTotal = distanciaTotal;
        this.tempoEstimado = tempoEstimado;
    }

    public String getOrigem() { return origem; }
    public String getDestino() { return destino; }
    public double getDistanciaTotal() { return distanciaTotal; }
    public double getTempoEstimado() { return tempoEstimado; }

    @Override
    public String toString() {
        return "Rota{origem='" + origem + "', destino='" + destino +
               "', distancia=" + distanciaTotal + "km, tempo=" + tempoEstimado + "h}";
    }
}


// Motorista.java
public class Motorista {
    private String nome;
    private String cpf;
    private String categoriaCNH;
    private String telefone;

    public Motorista(String nome, String cpf, String categoriaCNH, String telefone) {
        this.nome = nome;
        this.cpf = cpf;
        this.categoriaCNH = categoriaCNH;
        this.telefone = telefone;
    }

    public String getNome() { return nome; }
    public String getCpf() { return cpf; }
    public String getCategoriaCNH() { return categoriaCNH; }
    public String getTelefone() { return telefone; }

    @Override
    public String toString() {
        return "Motorista{nome='" + nome + "', CPF='" + cpf +
               "', CNH='" + categoriaCNH + "', tel='" + telefone + "'}";
    }
}




// Entrega.java
public class Entrega {
    private String codigo;
    private String nomeDestinatario;
    private String enderecoEntrega;
    private String dataEntrega;
    private String status; // "pendente", "atribuida", "finalizada"
    private double pesoCarga;
    private Rota rota;
    private Veiculo veiculo; // null se pendente

    public Entrega(String codigo, String nomeDestinatario, String enderecoEntrega,
                   String dataEntrega, double pesoCarga, Rota rota) {
        this.codigo = codigo;
        this.nomeDestinatario = nomeDestinatario;
        this.enderecoEntrega = enderecoEntrega;
        this.dataEntrega = dataEntrega;
        this.pesoCarga = pesoCarga;
        this.rota = rota;
        this.status = "pendente";
        this.veiculo = null;
    }

    public String getCodigo() { return codigo; }
    public String getNomeDestinatario() { return nomeDestinatario; }
    public String getEnderecoEntrega() { return enderecoEntrega; }
    public String getDataEntrega() { return dataEntrega; }
    public String getStatus() { return status; }
    public double getPesoCarga() { return pesoCarga; }
    public Rota getRota() { return rota; }
    public Veiculo getVeiculo() { return veiculo; }

    public void setStatus(String status) { this.status = status; }
    public void setVeiculo(Veiculo veiculo) { this.veiculo = veiculo; }

    @Override
    public String toString() {
        String veiculoInfo = (veiculo != null) ? veiculo.getPlaca() : "nenhum";
        return "Entrega{codigo='" + codigo + "', destinatario='" + nomeDestinatario +
               "', endereco='" + enderecoEntrega + "', data='" + dataEntrega +
               "', status='" + status + "', peso=" + pesoCarga + "kg" +
               ", veiculo=" + veiculoInfo + ", rota=" + rota + "}";
    }
}


// Veiculo.java
import java.util.ArrayList;
import java.util.List;

public class Veiculo {
    private String placa;
    private String modelo;
    private int anoFabricacao;
    private double capacidadeCarga;
    private String tipoVeiculo;
    private Motorista motorista;
    private List<Entrega> entregas; // apenas pendentes e atribuídas

    public Veiculo(String placa, String modelo, int anoFabricacao,
                   double capacidadeCarga, String tipoVeiculo) {
        this.placa = placa;
        this.modelo = modelo;
        this.anoFabricacao = anoFabricacao;
        this.capacidadeCarga = capacidadeCarga;
        this.tipoVeiculo = tipoVeiculo;
        this.motorista = null;
        this.entregas = new ArrayList<>();
    }

    public String getPlaca() { return placa; }
    public String getModelo() { return modelo; }
    public int getAnoFabricacao() { return anoFabricacao; }
    public double getCapacidadeCarga() { return capacidadeCarga; }
    public String getTipoVeiculo() { return tipoVeiculo; }
    public Motorista getMotorista() { return motorista; }
    public List<Entrega> getEntregas() { return entregas; }

    // Funcionalidade 1: Associar motorista ao veículo
    public void associarMotorista(Motorista motorista) {
        this.motorista = motorista;
        System.out.println("Motorista " + motorista.getNome() +
                           " associado ao veículo " + placa);
    }

    // Funcionalidade 2: Atribuir entrega ao veículo
    public void atribuirEntrega(Entrega entrega) {
        // Regra: entrega pendente não tem veículo; ao atribuir vira "atribuida"
        if (!entrega.getStatus().equals("pendente")) {
            System.out.println("Entrega " + entrega.getCodigo() +
                               " não está pendente. Status atual: " + entrega.getStatus());
            return;
        }
        entrega.setVeiculo(this);
        entrega.setStatus("atribuida");
        entregas.add(entrega);
        System.out.println("Entrega " + entrega.getCodigo() +
                           " atribuída ao veículo " + placa);
    }

    // Funcionalidade 3: Exibir todas as entregas do veículo + total
    public void exibirEntregas() {
        System.out.println("\n=== Entregas do veículo " + placa + " ===");
        if (entregas.isEmpty()) {
            System.out.println("Nenhuma entrega ativa.");
        } else {
            for (Entrega e : entregas) {
                System.out.println(e);
            }
        }
        System.out.println("Total de entregas ativas: " + entregas.size());
    }

    // Funcionalidade 4: Quantidade de entregas finalizadas
    // (o veículo não guarda referência às finalizadas, mas podemos receber a lista geral)
    public int contarEntregasFinalizadas(List<Entrega> todasEntregas) {
        int count = 0;
        for (Entrega e : todasEntregas) {
            if (e.getStatus().equals("finalizada") &&
                e.getVeiculo() != null &&
                e.getVeiculo().getPlaca().equals(this.placa)) {
                count++;
            }
        }
        return count;
    }

    // Finalizar entrega: remove da lista do veículo (veículo não mantém referência)
    public void finalizarEntrega(Entrega entrega) {
        if (entregas.contains(entrega)) {
            entregas.remove(entrega);
            entrega.setStatus("finalizada");
            // entrega mantém referência ao veículo, mas veículo remove da sua lista
            System.out.println("Entrega " + entrega.getCodigo() + " finalizada.");
        } else {
            System.out.println("Entrega não encontrada neste veículo.");
        }
    }

    @Override
    public String toString() {
        String motoristaInfo = (motorista != null) ? motorista.getNome() : "nenhum";
        return "Veiculo{placa='" + placa + "', modelo='" + modelo +
               "', ano=" + anoFabricacao + ", capacidade=" + capacidadeCarga +
               "kg, tipo='" + tipoVeiculo + "', motorista=" + motoristaInfo + "}";
    }
}


// SistemaLogistica.java
import java.util.ArrayList;
import java.util.List;

public class SistemaLogistica {
    private List<Veiculo> veiculos;
    private List<Motorista> motoristas;
    private List<Entrega> todasEntregas;

    public SistemaLogistica() {
        this.veiculos = new ArrayList<>();
        this.motoristas = new ArrayList<>();
        this.todasEntregas = new ArrayList<>();
    }

    public void adicionarVeiculo(Veiculo v) { veiculos.add(v); }
    public void adicionarMotorista(Motorista m) { motoristas.add(m); }
    public void adicionarEntrega(Entrega e) { todasEntregas.add(e); }

    // Funcionalidade 5: Buscar entregas por status
    public void buscarEntregasPorStatus(String status) {
        System.out.println("\n=== Entregas com status: " + status + " ===");
        boolean encontrou = false;
        for (Entrega e : todasEntregas) {
            if (e.getStatus().equalsIgnoreCase(status)) {
                encontrou = true;
                System.out.println(e);
                if (e.getVeiculo() != null) {
                    System.out.println("  Veículo: " + e.getVeiculo());
                    if (e.getVeiculo().getMotorista() != null) {
                        System.out.println("  Motorista: " + e.getVeiculo().getMotorista());
                    }
                }
            }
        }
        if (!encontrou) System.out.println("Nenhuma entrega encontrada.");
    }

    // Funcionalidade 6: Exibir detalhes completos de uma entrega
    public void exibirDetalhesEntrega(String codigo) {
        System.out.println("\n=== Detalhes da entrega: " + codigo + " ===");
        for (Entrega e : todasEntregas) {
            if (e.getCodigo().equals(codigo)) {
                System.out.println(e);
                return;
            }
        }
        System.out.println("Entrega não encontrada.");
    }

    // Funcionalidade 4: Relatório de finalizadas por veículo
    public void relatorioEntregasFinalizadas() {
        System.out.println("\n=== Entregas Finalizadas por Veículo ===");
        for (Veiculo v : veiculos) {
            int total = v.contarEntregasFinalizadas(todasEntregas);
            System.out.println("Veículo " + v.getPlaca() + ": " + total + " entrega(s) finalizada(s)");
        }
    }

    public List<Veiculo> getVeiculos() { return veiculos; }
    public List<Entrega> getTodasEntregas() { return todasEntregas; }
}




// Main.java
public class Main {
    public static void main(String[] args) {

        SistemaLogistica sistema = new SistemaLogistica();

        // --- Criar 3 motoristas ---
        Motorista m1 = new Motorista("Carlos Silva", "111.222.333-44", "C", "31-99001-0001");
        Motorista m2 = new Motorista("Ana Souza",   "555.666.777-88", "D", "31-99002-0002");
        Motorista m3 = new Motorista("Pedro Lima",  "999.000.111-22", "E", "31-99003-0003");
        sistema.adicionarMotorista(m1);
        sistema.adicionarMotorista(m2);
        sistema.adicionarMotorista(m3);

        // --- Criar 3 veículos ---
        Veiculo v1 = new Veiculo("ABC-1234", "Fiat Ducato",   2020, 1500.0, "Van");
        Veiculo v2 = new Veiculo("DEF-5678", "Mercedes Axor", 2019, 8000.0, "Caminhão");
        Veiculo v3 = new Veiculo("GHI-9012", "VW Delivery",   2021, 3500.0, "Truck");
        sistema.adicionarVeiculo(v1);
        sistema.adicionarVeiculo(v2);
        sistema.adicionarVeiculo(v3);

        // --- Funcionalidade 1: Associar motoristas aos veículos ---
        System.out.println("=== ASSOCIANDO MOTORISTAS ===");
        v1.associarMotorista(m1);
        v2.associarMotorista(m2);
        v3.associarMotorista(m3);

        // --- Criar rotas ---
        Rota r1 = new Rota("Belo Horizonte", "São Paulo",      586.0, 6.5);
        Rota r2 = new Rota("Belo Horizonte", "Rio de Janeiro", 434.0, 5.0);
        Rota r1b = new Rota("Belo Horizonte", "São Paulo",     586.0, 6.5); // mesma rota r1
        Rota r3 = new Rota("Belo Horizonte", "Brasília",       741.0, 8.0);
        Rota r4 = new Rota("São Paulo",      "Curitiba",       408.0, 4.5);

        // --- Criar entregas ---
        Entrega e1 = new Entrega("E001", "João Pereira",  "Av. Paulista, 100 - SP",  "2026-04-15", 200.0, r1);
        Entrega e2 = new Entrega("E002", "Maria Oliveira","Rua Copacabana, 50 - RJ", "2026-04-16", 350.0, r2);
        Entrega e3 = new Entrega("E003", "Lucas Ramos",   "Av. W3 Norte, 200 - BSB", "2026-04-17", 500.0, r3);
        Entrega e4 = new Entrega("E004", "Fernanda Costa","Rua XV, 30 - Curitiba",   "2026-04-18", 120.0, r4);
        Entrega e5 = new Entrega("E005", "Ricardo Alves", "Av. Ipiranga, 500 - SP",  "2026-04-20", 300.0, r1b);
        sistema.adicionarEntrega(e1);
        sistema.adicionarEntrega(e2);
        sistema.adicionarEntrega(e3);
        sistema.adicionarEntrega(e4);
        sistema.adicionarEntrega(e5);

        // --- Funcionalidade 2: Atribuir entregas aos veículos ---
        System.out.println("\n=== ATRIBUINDO ENTREGAS ===");
        v1.atribuirEntrega(e1);
        v1.atribuirEntrega(e5); // v1 tem 2 entregas
        v2.atribuirEntrega(e2);
        v2.atribuirEntrega(e3);
        v3.atribuirEntrega(e4);
        // e5 fica pendente propositalmente para demonstrar busca por status
        // (já foi atribuída acima — vamos deixar e4 como demonstração de pendente)

        // --- Funcionalidade 3: Exibir entregas por veículo ---
        System.out.println("\n=== EXIBINDO ENTREGAS POR VEÍCULO ===");
        v1.exibirEntregas();
        v2.exibirEntregas();
        v3.exibirEntregas();

        // --- Finalizar algumas entregas para demonstrar funcionalidade 4 ---
        System.out.println("\n=== FINALIZANDO ENTREGAS ===");
        v1.finalizarEntrega(e1);
        v2.finalizarEntrega(e2);
        v2.finalizarEntrega(e3);

        // --- Funcionalidade 4: Relatório de finalizadas por veículo ---
        sistema.relatorioEntregasFinalizadas();

        // --- Funcionalidade 5: Buscar por status ---
        sistema.buscarEntregasPorStatus("atribuida");
        sistema.buscarEntregasPorStatus("finalizada");
        sistema.buscarEntregasPorStatus("pendente");

        // --- Funcionalidade 6: Detalhes de entrega específica ---
        sistema.exibirDetalhesEntrega("E003");
        sistema.exibirDetalhesEntrega("E001");
    }
}




