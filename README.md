# Demostración de llamado de variantes con FreeBayes: FTO rs9939609

Este ejemplo didáctico muestra el concepto básico del llamado de variantes utilizando una variante genética humana real asociada con obesidad y diabetes tipo 2.

## Antecedentes biológicos

Este ejercicio está centrado en rs9939609, una de las variantes más famosas identificadas mediante estudios de asociación del genoma completo (GWAS).

### Gen: FTO

FTO (*Fat Mass and Obesity-Associated*) fue uno de los primeros loci asociados de manera robusta con el índice de masa corporal (IMC) y la obesidad.

### Variante: rs9939609

- **dbSNP ID:** rs9939609
- **Gen:** FTO
- **Contexto genómico:** Intrón 1
- **Alelo de referencia:** T
- **Alelo alternativo:** A

### Relevancia clínica

El alelo A se asocia con mayor índice de masa corporal, mayor riesgo de obesidad y mayor riesgo de diabetes tipo 2.

## Objetivo didáctico

1. Las lecturas se alinean contra una secuencia de referencia.
2. Se cuenta cuántas lecturas apoyan cada alelo.
3. Un modelo estadístico infiere el genotipo.
4. El resultado se almacena en un archivo VCF.

## Diseño del conjunto de datos sintético

La secuencia de referencia contiene 121 nucleótidos del locus de FTO alrededor de rs9939609.

La variante se localiza en la posición 61:

- Alelo de referencia: T
- Alelo alternativo: A

Cinco lecturas sintéticas fueron generadas:

- 2 lecturas contienen T
- 3 lecturas contienen A

Genotipo esperado:

- `0/1` (heterocigoto)

## Archivos incluidos

- `reference.fa`
- `sample.sam`
- `expected.vcf`


## Archivos generados

- `reference.fa.fai`
- `sample.bam`
- `sample.bam.bai`
- `sample.vcf`

## Herramientas necesarias

- [samtools](http://www.htslib.org/)
- [FreeBayes](https://github.com/freebayes/freebayes)


## Conversión de SAM a BAM

```bash
samtools faidx reference.fa
samtools view -bS sample.sam | samtools sort -o sample.bam
samtools index sample.bam
```

## Ejecución de FreeBayes

```bash
freebayes -f reference.fa sample.bam > sample.vcf
```

## Visualización del resultado

```bash
grep -v '^##' sample.vcf
```

## Resultado esperado

```text
#CHROM          POS  REF ALT QUAL
FTO_rs9939609   61   T   A   44.7557
```

```text
GT:DP:AD:RO:QR:AO:QA:GL
0/1:5:2,3:2:80:3:120:-9.68275,0,-6.08664
```

## Interpretación

- `GT = 0/1`: heterocigoto T/A.
- `DP = 5`: cinco lecturas cubren la posición.
- `AD = 2,3`: dos lecturas apoyan T y tres apoyan A.
- `RO = 2`: observaciones del alelo de referencia.
- `AO = 3`: observaciones del alelo alternativo.
- `AB = 0.6`: el 60% de las lecturas apoyan el alelo alternativo.

## Ilustración conceptual

```text
Referencia: T
Lecturas:   T T A A A
```

## Preguntas para discusión

1. ¿Por qué el genotipo es heterocigoto?
2. ¿Qué ocurriría si las cinco lecturas contuvieran A?
3. ¿Qué ocurriría si solo una lectura contuviera A?
4. ¿Por qué muchas variantes asociadas con enfermedad se encuentran fuera de regiones codificantes?

## Referencia

Frayling TM et al. (2007).
*A common variant in the FTO gene is associated with body mass index and predisposes to childhood and adult obesity.*
Science 316:889–894.
https://doi.org/10.1126/science.1141634

## Mensaje clave

El llamado de variantes transforma lecturas de secuenciación en genotipos clínicamente interpretables.
