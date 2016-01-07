# CATH Tools: 2DSEC

Provides a summary of a multiple structure alignment in terms of conservation of secondary structures.

## Usage

    $ 2DSEC <input alignment file> <output postscript file>

e.g. the files in the `example` directory were generated with:

    $ ./bin/2DSEC example/alignment1.cora example/alignment1.ps
    $ convert -rotate 90 -density 300 alignment1.ps -resize 1024x1024 alignment1.png

## Example

![Example 2DSEC image](/../screenshots/alignment1.png)

Note: 2DSEC outputs an image in PostScript. The example above uses the standard ImageMagick tool `convert` to convert this PostScript image to the more widely used PNG. Change the `-resize` parameter for a larger/smaller image.

## Alignment Format (CORA)

2DSEC was originally written as an internal tool for the [CATH protein structure classification database](http://www.cathdb.info). The algorithm reads the multiple structure alignments (MSA) created by our own alignment algorithm (CORA). This algorithm adds per-residue and per-alignment position information to the alignment which 2DSEC uses to build the schematic figure.

So, if you want to use the 2DSEC tool for your own work, you will need to convert your alignments to the CORA format.

The [example directory](./example) contains live data, see below for the full specification of the alignment format:


		Output File Format: CORA Alignment
		----------------------------------
		Example of the top of a CORA alignment file (CORA_FORMAT 1.1)
		#FM CORA_FORMAT 1.1
		#CC
		#CC Any number of comment lines (200 characters max per line)
		#CC
		#CC
		3
		6insE0 1igl00 1bqt00
		73
			 1    0    1    0  0  0    1  A  0    0  0  0   0    0    0   0
			 2    0    1    0  0  0    2  Y  0    0  0  0   0    0    0   0
			 3    0    2    1B F  0    3  R  0    0  0  0   0    0    0   0
			 4    0    3    2B V  H    4  P  0    1  G  0   0    1    0   2
			 5    1    3    3B N  H    5  S  0    2  P  0   0    1    0   6
			 6    0    3    4B Q  H    6  E  0    3  E  0   0    1    0   2

		CORA_FORMAT 1.1 has two sections:

		1. HEADER - File information

		One format line '#FM CORA_FORMAT 1.1'
		Any number of comment lines '#CC'
		Three lines of alignment information
			1. Total number of proteins in the alignment
			2. All CATH domain names in the alignment (6 characters + space)
			3. Total number of alignment positions

		2. BODY - Columns of alignment data

				 START       PROT 1     PROT 2     PROT N         END
		<------------><---------><---------><---------><---------------->
		ddddxddddxddddxddddcxcxxcxddddcxcxxcxddddcxcxxcxxxcxddddxddddxxdd

			 1    0    1    0  0  0    1  A  0    0  0  0   0    0    0   0
			 2    0    1    0  0  0    2  Y  0    0  0  0   0    0    0   0
			 3    0    2    1B F  0    3  R  0    0  0  0   0    0    0   0
			 4    0    3    2B V  H    4  P  0    1  G  0   0    1    0   2
			 5    1    3    3B N  H    5  S  0    2  P  0   0    1    0   6
			 6    0    3    4B Q  H    6  E  0    3  E  0   0    1    0   2

		START (14 characters)
			Column 1: Alignment Position (dddd)
			Column 2: No. of position selected for structural template (dddd)
			Column 3: No. of proteins aligned at this position (dddd)

		PROT 1,2,3... (11 characters per protein)
			Column 4 (7,10 etc): Residue number in PDB file (ddddc) 4 digit number
				 + 1 character insert code
				 Importantly the insert code is always in the same position not within
				 the 4 characters reserved for the pdb number (see below)
			Column 5 (8,11 etc): Amino Acid Code (c)
			Column 6 (9,12 etc): Secondary Structure Assignment (c)

		END (18 characters)
			Last Column-3: Consensus Secondary Structure Assignment (c)
			Last Column-2: No. of alpha residues at this position (dddd)
			Last Column-1: No. of beta residues at this position (dddd)
			Last Column: Structural Conservation Score (dd)
