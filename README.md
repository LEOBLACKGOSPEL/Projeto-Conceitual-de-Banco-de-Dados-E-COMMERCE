-- Criando o banco de dados
CREATE DATABASE ecommerce;
USE ecommerce;

-- Tabela de Clientes
CREATE TABLE Cliente (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    telefone VARCHAR(20),
    tipo ENUM('PJ', 'PF') NOT NULL,
    cpf_cnpj VARCHAR(18) UNIQUE NOT NULL,
    endereco TEXT
);

-- Tabela de Produtos
CREATE TABLE Produto (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    descricao TEXT,
    preco DECIMAL(10,2) NOT NULL,
    categoria VARCHAR(100),
    fornecedor_id INT,
    FOREIGN KEY (fornecedor_id) REFERENCES Fornecedor(id)
);

-- Tabela de Estoque
CREATE TABLE Estoque (
    id INT AUTO_INCREMENT PRIMARY KEY,
    produto_id INT NOT NULL,
    quantidade INT NOT NULL,
    localizacao VARCHAR(255),
    FOREIGN KEY (produto_id) REFERENCES Produto(id)
);

-- Tabela de Pedidos
CREATE TABLE Pedido (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT NOT NULL,
    data_pedido DATETIME DEFAULT CURRENT_TIMESTAMP,
    status ENUM('Pendente', 'Processando', 'Enviado', 'Entregue', 'Cancelado') NOT NULL,
    codigo_rastreamento VARCHAR(50),
    FOREIGN KEY (cliente_id) REFERENCES Cliente(id)
);

-- Tabela de Pagamentos
CREATE TABLE Pagamento (
    id INT AUTO_INCREMENT PRIMARY KEY,
    pedido_id INT NOT NULL,
    metodo ENUM('Cartão de Crédito', 'Boleto', 'Pix', 'PayPal') NOT NULL,
    status ENUM('Aprovado', 'Pendente', 'Recusado') NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES Pedido(id)
);

-- Tabela de Itens do Pedido
CREATE TABLE Item_Pedido (
    id INT AUTO_INCREMENT PRIMARY KEY,
    pedido_id INT NOT NULL,
    produto_id INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES Pedido(id),
    FOREIGN KEY (produto_id) REFERENCES Produto(id)
);

-- Tabela de Fornecedores
CREATE TABLE Fornecedor (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    contato VARCHAR(255),
    telefone VARCHAR(20),
    endereco TEXT
);

