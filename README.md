#28/09

#---------------------------------------------------------------------------------------------------

select * from logradouro;

select * from logradouro where cep = "89218480";

select * from bairros where cd_bairro = "14033";

select ds_logradouro_nome, ds_bairro_nome, ds_cidade_nome, ds_uf_nome, ds_uf_sigla
from logradouro, bairros, cidades, uf
where cep = "89218480" and 
bairros_cd_bairro = cd_bairro and 
cidade_cd_cidade = cd_cidade and
uf_cd_uf = cd_uf;

select * from cad_usuario;

select nome, ds_logradouro_nome, ds_bairro_nome, ds_cidade_nome, ds_uf_nome, ds_uf_sigla
from logradouro, bairros, cidades, uf, cad_usuario
where
bairros_cd_bairro = cd_bairro and 
cidade_cd_cidade = cd_cidade and
uf_cd_uf = cd_uf and
log_cd_logradouro = cd_logradouro;

select * from pedidos;

select cad_usuario_cpf from pedidos
;

select nome, cod_pedido from cad_usuario, pedidos
where
cpf = cad_usuario_cpf;

select nome, cod_pedido, dtped, qtditem, descricao, preco_unit 
from cad_usuario, pedidos, itemped, produto
where cad_usuario.cpf = pedidos.cad_usuario_cpf and
pedidos.cod_pedido = itemped.ped_codpedidos and
itemped.prod_cod_produto = produto.cod_produto;

select nome, cpf, descricao_tipo, ds_logradouro_nome 
from cad_usuario, tipo_usuario, logradouro
where cad_usuario.tipuser_cd = tipo_usuario.cod_tip_user 
and cad_usuario.log_cd_logradouro = logradouro.cd_logradouro;

#---------------------------------------------------------------------------------------------------

#29/07

#---------------------------------------------------------------------------------------------------

select * from pedidos;

select nome, email, tel, cod_pedido, dtped, qtditem, descricao, preco_unit 
from cad_usuario, pedidos, itemped, produto
where cad_usuario.cpf = pedidos.cad_usuario_cpf
and produto.cod_produto = itemped.prod_cod_produto
and pedidos.cod_pedido = itemped.ped_codpedidos
#and cad_usuario.cpf = '63748762435'
;

select * from tipo_logradouro;

select ds_logradouro_nome, ds_bairro_nome, ds_cidade_nome
from logradouro, bairros, cidades, tipo_logradouro
where logradouro.bairros_cd_bairro = bairros.cd_bairro
and bairros.cidade_cd_cidade = cidades.cd_cidade
and logradouro.log_cd_tip_log = tipo_logradouro.cd_tipo_logradouro 
and tipo_logradouro.desc_tip_log = 'praça'
#and cidades.ds_cidade_nome = 'joinville'
;

select cod_pedido, cad_usuario_cpf, dtped, faturado, naofaturado, dtentrega, qtditem, descricao, preco_unit
from itemped, produto, pedidos
where produto.preco_unit >= 1.2
and produto.preco_unit <= 8
and itemped.prod_cod_produto = produto.cod_produto
and pedidos.cod_pedido = itemped.ped_codpedidos;

select * from pedidos;

#---------------------------------------------------------------------------------------------------

#05/10

#---------------------------------------------------------------------------------------------------

select * from produto; /* Como fazer comentário em bloco */

#---------------------------------------------------------------------------------------------------

#Exercícios
#1) Cruc que retorne: descricao, preco_unit acrescido de R$ 0.75
#2) Cruc que retorne: descricao, preco_unit, preco_emb, dos produtos com qtd_emb menor igual a 12
#3)Cruc que retorne descricao, preco_unit e preco_unit acrescido de 0.5%

#---------------------------------------------------------------------------------------------------

select descricao, preco_unit + 0.75 as resultado from produto;

select cod_produto, descricao, preco_unit, preco_emb, qtd_emb
from produto;

update produto set preco_emb = preco_unit * qtd_emb where cod_produto = 22;		

update produto set preco_emb = preco_unit * qtd_emb
where preco_emb <> preco_unit * qtd_emb;																																																				

select descricao, preco_unit, preco_emb
from produto
where produto.qtd_emb <= 12;

select descricao, preco_unit * 1.005 as preco_atualizado
from produto;

#---------------------------------------------------------------------------------------------------

#19/10

#---------------------------------------------------------------------------------------------------

select * from produto;

select max(preco_unit) from produto;

select min(preco_unit) from produto;

select avg(preco_unit) from produto;

select count(cod_produto) from produto;

select sum(preco_unit) from produto;

select descricao, preco_unit 
from produto
where preco_unit = (select max(preco_unit) from produto);

select nome, descricao_tipo
from cad_usuario, tipo_usuario
where cad_usuario.tipuser_cd = tipo_usuario.cod_tip_user;

#select count(tipuser_cd)
#from cad_usuario, tipo_usuario
#where cad_usuario.tipuser_cd = tipo_usuario.cod_tip_user;

select descricao_tipo, count(cod_tip_user) as codigo_tipo
from cad_usuario, tipo_usuario
where cad_usuario.tipuser_cd = tipo_usuario.cod_tip_user
group by descricao_tipo;

select descricao_tipo as Tipo_usuario, count(cod_tip_user) as numero
from cad_usuario, tipo_usuario
where cad_usuario.tipuser_cd = tipo_usuario.cod_tip_user
group by descricao_tipo order by descricao_tipo desc; #desc -> maior para o menor; asc -> menor para o maior

select round(avg(preco_unit),4) from produto;

#---------------------------------------------------------------------------------------------------

#Exercícios

#1) CRUC que retorne: nome dos usuários e a quantidade de pedidos realizados por cada usuário.

#2) CRUC que retorne: nome do usuário e o valor total do pedido, filtrando por um código de pedido válido.

#3) CRUC que retorne: a quantidade de logradouros filtrando pela sigla do estado.

#4) CRUC que retorne: a quantidade de bairros agrupando pela sigla do estado.

#5) CRUC que retorne: preço total dos pedidos agrupando pelo código dos pedidos.

#---------------------------------------------------------------------------------------------------

#select cad_usuario.nome as username, (itemped.qtditem) as qtd_pedido
#from cad_usuario, itemped, pedidos
#where itemped.ped_codpedidos = pedidos.cod_pedido;

select cad_usuario.nome as username, sum(itemped.qtditem) as qtd_pedido
from cad_usuario, itemped, pedidos
where itemped.ped_codpedidos = pedidos.cod_pedido
#and cad_usuario.cpf =  pedidos.cad_usuario_cpf
group by username;

#select cad_usuario.nome, produto.preco_unit
#from cad_usuario, pedidos, itemped, produto
#where cad_usuario.cpf = pedidos.cad_usuario_cpf
#and pedidos.cod_pedido = itemped.ped_codpedidos
#and itemped.prod_cod_produto = produto.cod_produto;

select cad_usuario.nome, sum(produto.preco_unit * itemped.qtditem) as preco_total
from cad_usuario, pedidos, itemped, produto
where cad_usuario.cpf = pedidos.cad_usuario_cpf
and pedidos.cod_pedido = itemped.ped_codpedidos
and itemped.prod_cod_produto = produto.cod_produto
group by nome;

select



