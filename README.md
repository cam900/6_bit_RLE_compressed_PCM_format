# 6 bit RLE compressed PCM format

This format is RLE compressible 6 bit DPCM, it's encoded to single byte for ease to use.

Each output is expanded to 16 bit signed PCM.

## Data format (single byte):

<table>
	<tr>
		<th colspan="9" align="center">Byte format</th>
	</tr>
	<tr>
		<th colspan="8" align="center">Bit</th>
		<th rowspan="2" align="left">Description</th>
	</tr>
	<tr>
		<td>7</td>
		<td>6</td>
		<td>5</td>
		<td>4</td>
		<td>3</td>
		<td>2</td>
		<td>1</td>
		<td>0</td>
	</tr>
	<tr>
		<td>x</td>
		<td>-</td> <td>-</td> <td>-</td> <td>-</td> <td>-</td> <td>-</td> <td>-</td>
		<td>Delta bit</td>
	</tr>
	<tr>
		<td>-</td>
		<td>0</td>
		<td>x</td> <td>x</td> <td>x</td> <td>x</td> <td>x</td> <td>x</td>
		<td>Sound output</td>
	</tr>
	<tr>
		<td>-</td> 
		<td>0</td> <td>x</td>
		<td>-</td> <td>-</td> <td>-</td> <td>-</td> <td>-</td>
		<td>Negative bit</td>
	</tr>
	<tr>
		<td>-</td>
		<td>0</td> <td>-</td>
		<td>x</td> <td>x</td> <td>x</td> <td>x</td> <td>x</td>
		<td>Output bit</td>
	</tr>
	<tr>
		<td>-</td>
		<td>1</td>
		<td>x</td> <td>x</td> <td>x</td> <td>x</td> <td>x</td> <td>x</td>
		<td>Repeat previous output update x - 1 time (RLE)</td>
	</tr>
</table>

<table>
	<tr>
		<th colspan="2" align="center">Delta bit</th>
	</tr>
	<tr>
		<th align="center">Bit</th>
		<th align="left">Description</th>
	</tr>
	<tr>
		<td>0</td>
		<td>Replace output</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Add to previous output</td>
	</tr>
</table>

<table>
	<tr>
		<th colspan="2" align="center">Negative bit</th>
	</tr>
	<tr>
		<th align="center">Bit</th>
		<th align="left">Description</th>
	</tr>
	<tr>
		<td>0</td>
		<td>Positive</td>
	</tr>
	<tr>
		<td>1</td>
		<td>Negative</td>
	</tr>
</table>

<table>
	<tr>
		<th colspan="6" align="center">Output bit</th>
	</tr>
	<tr>
		<th colspan="5" align="center">Bit</th>
		<th rowspan="2" align="left">Description</th>
	</tr>
	<tr>
		<td>4</td>
		<td>3</td>
		<td>2</td>
		<td>1</td>
		<td>0</td>
	</tr>
	<tr>
		<td>e</td> <td>e</td> <td>e</td> <td>e</td>
		<td>m</td>
		<td>Output = (2 + m) << (e - 1)</td>
	</tr>
	<tr>
		<td>0</td> <td>0</td> <td>0</td>
		<td>x</td> <td>x</td>
		<td>Output = x</td>
	</tr>
</table>


### Decode routine

	X = (X-1 * D) + (Y * 1 - (Neg * 2))
	X = -32767 < X < 32767

	X = Current output
	X-1 = Previous output
	D = Delta bit
	Y = Next output
	Neg = Negative bit
