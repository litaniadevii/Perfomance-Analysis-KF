create table kimia_farma.table_analysis1 as
  select
kf_final_transaction.transaction_id 
kf_final_transactiondate, kf_final_transaction.branch_id, kf_final_transaction.customer_name, kf_final_transaction.product_id, kf_final_transaction.discount_percentage, kf_final_transaction.rating as rating_transaksi, kf_inventory.product_name, kf_kantor_cabang.branch_name, kf_kantor_cabang.kota, kf_kantor_cabang.provinsi, kf_kantor_cabang.rating, kf_product.price as actual_price
from kimia_farma.kf_product, kimia_farma.kf_kantor_cabang, kimia_farma.kf_inventory, kimia_farma.kf_final_transaction
;
create table kimia_farma.table_analysis2 as
select
    kf_product.price,
    case
        when kf_product.price <= 50000 then kf_product.price * 0.1
        when kf_product.price > 50000 and kf_product.price <= 100000 then kf_product.price * 0.15
        when kf_product.price > 100000 and kf_product.price <= 300000 then kf_product.price * 0.2
        when kf_product.price > 300000 and kf_product.price <= 500000 then kf_product.price * 0.25
        else kf_product.price * 0.3
    end as persentase_gross_laba
from kimia_farma.kf_product
;
create table kimia_farma.table_analysis3 as
select *,
      (kf_final_transaction.price-(kf_final_transaction.price * kf_final_transaction.discount_percentage)) as nett_sales
      from kimia_farma.kf_final_transaction
;
create table kimia_farma.table_analysis4 as
select provinsi, sum (price-(price*discount_percentage)) as nett_profit
from kimia_farma.kf_final_transaction, kimia_farma.kf_kantor_cabang
group by provinsi
;
