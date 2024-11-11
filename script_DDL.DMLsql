-- deletar banco 
DROP DATABASE EcommerceFacul;

-- Criamos o banco de dados se ele não existir
CREATE DATABASE IF NOT EXISTS EcommerceFacul;

-- Utilizamos o banco de dados criado
USE EcommerceFacul;

-- 1  A tabela DadosCadastrais armazena informações de contato e identificação dos clientes
CREATE TABLE DadosCadastrais(
    idDadosCadastrais INT PRIMARY KEY AUTO_INCREMENT NOT NULL,  -- idDadosCadastrais é a chave primária.
    Email VARCHAR(100) NOT NULL,  -- Email do cliente.
    DataCadastro DATE NOT NULL,  -- Data em que o cliente se cadastrou.
    CPF VARCHAR(11) UNIQUE NOT NULL,  -- CPF do cliente, 11 dígitos, precisa ser único.
    Telefone VARCHAR(20) NOT NULL,  -- Telefone do cliente.
    CNPJ CHAR(14) UNIQUE NOT NULL,  -- CNPJ é único e tem 14 dígitos (agora CHAR(14)).
    Endereço VARCHAR(100) NOT NULL,  -- Endereço completo do cliente.
    CEP VARCHAR(8) NOT NULL,  -- CEP (código postal) de 8 dígitos.
    RUA VARCHAR(50) NOT NULL,  -- Nome da rua com até 50 caracteres.
    ESTADO VARCHAR(50),  -- Estado onde o cliente reside.
    idFornecedor INT  -- Chave estrangeira opcional para referenciar fornecedores.
);

-- 2  A tabela Categoria classifica os produtos.
CREATE TABLE Categoria(
    idCategoria INT PRIMARY KEY AUTO_INCREMENT NOT NULL,  -- idCategoria é a chave primária.
    NomeCategoria VARCHAR(50) NOT NULL  -- Nome da categoria de produto, até 50 caracteres.
);

-- 3 A tabela Fornecedor guarda as informações dos fornecedores de produtos.
CREATE TABLE Fornecedor(
    idFornecedor INT PRIMARY KEY AUTO_INCREMENT NOT NULL,  -- idFornecedor é a chave primária.
    Nomefornecedor VARCHAR(60) NOT NULL,  -- Nome do fornecedor, até 60 caracteres.
    CNPJ CHAR(14) UNIQUE NOT NULL,  -- CNPJ único do fornecedor, 14 dígitos.
    Telefone VARCHAR(20) NOT NULL  -- Telefone do fornecedor.
);

-- A tabela Produto armazena detalhes sobre cada produto.
CREATE TABLE Produto(
    idProduto INT PRIMARY KEY AUTO_INCREMENT NOT NULL,  -- idProduto é a chave primária.
    NomeProduto VARCHAR(50) NOT NULL,  -- Nome do produto.
    Preço DECIMAL(10,2) NOT NULL,  -- Preço do produto com 2 casas decimais.
    idCategoria INT,  -- Chave estrangeira para a categoria do produto.
    idFornecedor INT,  -- Chave estrangeira para o fornecedor do produto.
    FOREIGN KEY (idCategoria) REFERENCES Categoria(idCategoria),  -- Relaciona com a tabela Categoria.
    FOREIGN KEY (idFornecedor) REFERENCES Fornecedor(idFornecedor)  -- Relaciona com a tabela Fornecedor.
);

-- tabela log 
CREATE TABLE Log(
idLog  INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
NomeTabela VARCHAR(50) NOT NULL,
Operacao VARCHAR(10) NOT NULL ,CHECK(operacao IN ('INSERT', 'UPDATE', 'DELETE')),
ValorAtual VARCHAR(500) , 
ValorNovo VARCHAR(500)
);

-- A tabela Cliente armazena informações dos clientes.
CREATE TABLE Cliente(
    idCliente INT PRIMARY KEY AUTO_INCREMENT NOT NULL,  -- idCliente é a chave primária, gerada automaticamente para cada cliente.
    NomeCliente VARCHAR(100) NOT NULL,  -- NomeCliente armazena o nome do cliente, até 100 caracteres.
    DataNascimento DATE NOT NULL,  -- Data de nascimento do cliente.
    idDadosCadastrais INT,  -- Chave estrangeira para os dados cadastrais.
    idPedido INT,  -- Chave estrangeira para os pedidos.
    FOREIGN KEY (idDadosCadastrais) REFERENCES DadosCadastrais(idDadosCadastrais),  -- Relaciona com a tabela DadosCadastrais.
    FOREIGN KEY (idPedido) REFERENCES Pedido(idPedido)  -- Relaciona com a tabela Pedido.
);

-- 4  A tabela Pedido armazena as informações dos pedidos feitos pelos clientes.
CREATE TABLE Pedido(
    idPedido INT PRIMARY KEY AUTO_INCREMENT NOT NULL,  -- idPedido é a chave primária.
    DataPedido DATE NOT NULL,  -- Data em que o pedido foi feito.
    ValorTotal DECIMAL(10,2) NOT NULL,  -- Valor total do pedido.
    StatusPagamento VARCHAR(100) NOT NULL,  -- Status do pagamento (pago, pendente, etc.).
    idCliente INT  -- Chave estrangeira para o cliente que fez o pedido.
    -- FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)  -- Relaciona com a tabela Cliente.
);

-- adicionando chave estrangeira entre cliente e pedido erro de chave no criava a tabela

ALTER TABLE Pedido
ADD CONSTRAINT fk_Pedido_Cliente
FOREIGN KEY (idCliente) REFERENCES Cliente (idCliente);

-- A tabela intermediária para relacionamento muitos-para-muitos entre pedidos e produtos.
CREATE TABLE PedidoProduto(
    idPedido INT,  -- Chave estrangeira para a tabela Pedido.
    idProduto INT,  -- Chave estrangeira para a tabela Produto.
    Quantidade INT NOT NULL,  -- Quantidade de produtos nesse pedido.
    PreçoUnitario DECIMAL(10,2) NOT NULL,  -- Preço unitário do produto no momento da compra.
    PRIMARY KEY (idPedido, idProduto),  -- A chave primária é a combinação do idPedido e idProduto.
    FOREIGN KEY (idPedido) REFERENCES Pedido(idPedido),  -- Relaciona com a tabela Pedido.
    FOREIGN KEY (idProduto) REFERENCES Produto(idProduto)  -- Relaciona com a tabela Produto.
);

-- A tabela Estoque armazena a quantidade disponível de cada produto.
CREATE TABLE Estoque(
    idEstoque INT PRIMARY KEY AUTO_INCREMENT NOT NULL,  -- idEstoque é a chave primária.
    idProduto INT,  -- Chave estrangeira para a tabela Produto.
    Quantidade INT NOT NULL,  -- Quantidade disponível no estoque.
    FOREIGN KEY (idProduto) REFERENCES Produto(idProduto)  -- Relaciona com a tabela Produto.
);

-- Criando índices para otimizar as consultas frequentes.
CREATE INDEX idx_NomeCliente ON Cliente (NomeCliente);  -- Índice para agilizar as buscas por nome de cliente.
CREATE INDEX idx_NomeProduto ON Produto (NomeProduto);  -- Índice para agilizar as buscas por nome de produto.
CREATE INDEX idx_DataPedido ON Pedido (DataPedido);  -- Índice para agilizar buscas por data de pedido.


-- Trigger para INSERT
-- Essa trigger será acionada sempre que uma nova linha for inserida em uma tabela ela irá registrar o nome da tabela e os dados inseridos.

DELIMITER $$
CREATE TRIGGER LogInsert AFTER INSERT ON Categoria -- aqui voce pode mudar o nome para a tabela deseja monitorar
FOR EACH ROW
BEGIN
	INSERT INTO Log(NomeTabela, Operacao, ValorAtual, ValorNovo)
    VALUES ('Categoria', 'INSERT', NULL, CONCAT('NomeCategoria: ', NEW.NomeCategoria));
END$$
DELIMITER $$

-- Trigger para UPDATE
-- Essa trigger será acionada sempre que um registro for atualizado. Ela irá registrar tanto os valores antigos (antes da atualização) quanto os valores novos (depois da atualização).

DELIMITER $$
CREATE TRIGGER LogUpdate AFTER UPDATE ON Categoria   -- colocar o nome tabela que deseja monitorar
FOR EACH ROW
BEGIN
	INSERT INTO Log(NomeTabela, Operacao, valorAtual, ValorNovo)
    VALUES (
		'Categoria',
        'UPDATE',
        CONCAT('NomeCategoria: ', OLD.NomeCategoria), -- valor antes
        CONCAT('NomeCategoria: ', NEW.NomeCategoria)); -- valor depois
END$$
DELIMITER $$

-- Trigger para DELETE
-- Essa trigger será acionada sempre que um registro for deletado. Ela irá registrar os valores antigos (antes da exclusão).
        
DELIMITER $$
CREATE TRIGGER LogDelete AFTER DELETE ON Categoria -- nome da tabela que quer monitorar
FOR EACH ROW 
BEGIN
	INSERT INTO Log(NomeTabela, Operacao, ValorAtual, ValorNovo)
    VALUES(
    'Categoria',
    'DELETE', 
    CONCAT('NomeTabela: ', OLD.NomeCategoria), -- antes da exclusao
    NULL -- sem valor pois foi deletado
	);
END$$
DELIMITER $$

-- Trigger para validação de CPF, garantindo que tenha 11 dígitos.
DELIMITER $$

DELIMITER $$
CREATE TRIGGER VerificarCPF
BEFORE INSERT ON DadosCadastrais
FOR EACH ROW
BEGIN
    DECLARE digito1, digito2, soma INT;

    -- Validar comprimento e formato do CPF (11 dígitos numéricos)
    IF CHAR_LENGTH(NEW.CPF) <> 11 OR NEW.CPF REGEXP '[^0-9]' THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'CPF deve conter 11 dígitos numéricos.';
    END IF;

    -- Calcular o primeiro dígito verificador
    SET soma = (SUBSTRING(NEW.CPF, 1, 1) * 10 +
                SUBSTRING(NEW.CPF, 2, 1) * 9 +
                SUBSTRING(NEW.CPF, 3, 1) * 8 +
                SUBSTRING(NEW.CPF, 4, 1) * 7 +
                SUBSTRING(NEW.CPF, 5, 1) * 6 +
                SUBSTRING(NEW.CPF, 6, 1) * 5 +
                SUBSTRING(NEW.CPF, 7, 1) * 4 +
                SUBSTRING(NEW.CPF, 8, 1) * 3 +
                SUBSTRING(NEW.CPF, 9, 1) * 2);
    SET digito1 = IF(soma % 11 < 2, 0, 11 - (soma % 11));

    -- Calcular o segundo dígito verificador
    SET soma = (SUBSTRING(NEW.CPF, 1, 1) * 11 +
                SUBSTRING(NEW.CPF, 2, 1) * 10 +
                SUBSTRING(NEW.CPF, 3, 1) * 9 +
                SUBSTRING(NEW.CPF, 4, 1) * 8 +
                SUBSTRING(NEW.CPF, 5, 1) * 7 +
                SUBSTRING(NEW.CPF, 6, 1) * 6 +
                SUBSTRING(NEW.CPF, 7, 1) * 5 +
                SUBSTRING(NEW.CPF, 8, 1) * 4 +
                SUBSTRING(NEW.CPF, 9, 1) * 3 +
                digito1 * 2);
    SET digito2 = IF(soma % 11 < 2, 0, 11 - (soma % 11));

    -- Verificar validade do CPF com os dígitos verificadores
    IF digito1 != SUBSTRING(NEW.CPF, 10, 1) OR digito2 != SUBSTRING(NEW.CPF, 11, 1) THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'CPF inválido.';
    END IF;
END$$
DELIMITER ;

-- scripty test dos log e cpf 

-- testando cpf
INSERT INTO DadosCadastrais (Email, DataCadastro, CPF, Telefone, CNPJ, Endereço, CEP, RUA, Estado, idFornecedor)
VALUES ('test@gmail.com', CURDATE(), '55712538865' , '99999999999', '12121212121212', 'rua sape', '07183060', 'Rua sape', 'SP',  1);
 
 -- ver tudo que tem na tabela
SELECT *FROM DadosCadastrais;


-- testando o LOG

-- teste inserçao tabela categoria
INSERT INTO Categoria (NomeCategoria) VALUES ('Eletronicos');

-- test update, atualizando registro de categoria
UPDATE Categoria SET NomeCategoria = 'Eletrodomesticos' WHERE NomeCategoria = 'Eletronicos';

-- test delete , delete de um campo na tabela categoria ou a qual necessitar
DELETE FROM Categoria WHERE NomeCategoria = 'Eletrodomesticos';




-- verifique log

SELECT *FROM Log;

SELECT *FROM DadosCadastrais;
