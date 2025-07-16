## 1. Identificar clientes que realizaram pedidos mas nunca usaram PIX como forma de pagamento.
```
SELECT nome, cpf FROM cliente AS c
JOIN pedido AS p ON p.cliente_id = c.id
WHERE p.forma_pagamento_id = 1;
```

## 2. Verificar quais produtos estão vendendo acima da média de vendas de sua categoria.
```
SELECT p.nome, AVG(ip.quantidade) AS MediaVendas, cat.nome FROM produto AS p
JOIN produto_categoria AS pc ON pc.produto_id = p.id
JOIN categoria AS cat ON cat.id = pc.categoria_id
JOIN item_pedido AS ip ON ip.produto_id = p.id
GROUP BY p.nome, cat.nome
```

## 3. Calcular o total de vendas entregues por cada transportadora.
```
SELECT nome, COUNT(e.id) AS TotalEntregas FROM transportadora AS t
JOIN entrega AS e ON e.transportadora_id = t.id
JOIN pedido AS p ON p.id = e.id
GROUP BY e.transportadora_id;
```

## 4. Identificar clientes cujos pedidos excederam R$ 1000.
```
SELECT c.nome, c.cpf, SUM(ip.quantidade * ip.preco_unitario) AS totalCompras 
FROM cliente AS c
JOIN pedido AS p ON p.cliente_id = c.id
JOIN item_pedido AS ip ON ip.pedido_id = p.id
GROUP BY c.nome, c.cpf
HAVING SUM(ip.quantidade * ip.preco_unitario) > 1000;
```

## 5. Encontrar produtos que nunca foram vendidos.
```
SELECT p.nome FROM produto AS p
LEFT JOIN item_pedido AS ip ON ip.produto_id = p.id
GROUP BY p.nome
HAVING SUM(ip.quantidade) IS NULL;
```

## 6. Comparar a atividade de pedidos no primeiro e último dia do mês.
```
SELECT * FROM pedido
WHERE data_pedido
???
```

## 7. Verificar detalhes dos pedidos que foram entregues após a data prevista.
```
SELECT * FROM pedido AS p
JOIN entrega AS e ON p.id = e.pedido_id
WHERE e.data_prevista < e.data_entrega;
```

## 8. Analisar a frequência de uso das diferentes formas de pagamento.
```
SELECT
  COUNT(IF(forma_pagamento_id = 1, 1, NULL)) AS PIX,
  COUNT(IF(forma_pagamento_id = 2, 1, NULL)) AS CartaoCredito
FROM pedido;
```
## 9. Gerar um relatório dos pedidos baseado no estado do cliente.
