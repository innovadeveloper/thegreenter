# Factura con Deducción de Anticipos

> Las lineas resaltadas son propias de este modelo de factura.

## Código

```php hl_lines="24 25 26 27 28 29 30"
<?php
use Greenter\Model\Sale\Invoice;
use Greenter\Model\Sale\Legend;
use Greenter\Model\Sale\Prepayment;
use Greenter\Model\Sale\SaleDetail;

$invoice = new Invoice();
$invoice
    ->setUblVersion('2.1')
    ->setTipoOperacion('0101')
    ->setTipoDoc('01')
    ->setSerie('F001')
    ->setCorrelativo('123')
    ->setFechaEmision(new \DateTime())
    ->setTipoMoneda('PEN')
    ->setClient($this->getClient())
    ->setCompany($this->getCompany())
    ->setMtoOperGravadas(200)
    ->setMtoIGV(36)
    ->setValorVenta(200)
    ->setTotalImpuestos(36)
    ->setSubTotal(236)
    ->setMtoImpVenta(136) // Disminución por anticipo
    ->setAnticipos([
        (new Prepayment())
            ->setTipoDocRel('02') // catalog. 12
            ->setNroDocRel('F001-111')
            ->setTotal(100)
    ])
    ->setTotalAnticipos(100);

$detail = new SaleDetail();
$detail->setCodProducto('P001')
    ->setUnidad('NIU')
    ->setDescripcion('PROD 1')
    ->setCantidad(2)
    ->setMtoValorUnitario(100)
    ->setMtoValorVenta(200)
    ->setMtoBaseIgv(200)
    ->setPorcentajeIgv(18)
    ->setIgv(36)
    ->setTipAfeIgv('10')
    ->setTotalImpuestos(36)
    ->setMtoPrecioUnitario(118)
;

$invoice->setDetails([$detail])
    ->setLegends([
        (new Legend())
            ->setCode('1000')
            ->setValue('SON CIENTO DIECIOCHO CON OO/100 SOLES')
    ]);
```