# SMIRKS Acid Test

SMIRKS patterns for testing portability and compliance

# Overview

Testing the limits, quirks and corner cases of
SMIRKS implementations. This provides a SMILES and 
SMIRKS to apply to it, tab separated line, one per line:

| SMILES | SMIRKS | COMMENT |
|--------|--------|---------|

Comments and blank lines should be echoed, otherwise 
you should run the SMIRKS against the SMILES once
and add an new column containing the output SMILES.
There are different ways to run a SMIRKS and depends
on the toolkits being tested.

| SMILES | SMIRKS | OUTPUT | COMMENT |
|--------|--------|--------|---------|

* If the SMILES is considered invalid, output ``<bad smiles>``
* If the SMIRKS is considered invalid, output ``<bad smirks>``
* If the SMIRKS did not apply, output ``<no match>``

# Example

| SMILES | SMIRKS                 | COMMENT |
|--------|------------------------|---------|
| ``C``  | ``[CH4:1]>>[CH3:1]-O`` |         |

```python
def output(smiin, smirks, smiout, comment):
  print ("%s\t%s\t%s\t%s" % (smiin, smirks, smiout, comment))

def run(smiles, smirks, comment):
  mol = Chem.MolFromSmiles(smiles)
  if not mol:
    output(smiles, smirks, "<bad smiles>", comment)
    continue
  try:
    rxn = AllChem.ReactionFromSmarts(smirks)
    results = rxn.RunReactants((mol,));
    if not results:
      output(smiles, smirks, "<no match>", comment)
    for x in results:		
      output(smiles, smirks, Chem.MolToSmiles(x[0],1), comment)
      break;
  except:
    output(smiles, smirks, "<bad smirks>", comment)
```

| SMILES | SMIRKS                 | OUTPUT | COMMENT |
|--------|------------------------|--------|---------|
| ``C``  | ``[CH4:1]>>[CH3:1]-O`` | ``CO`` |         |




