# pivot_table: simple pivot table creator

Create pivot table from column table (just only output first value, no arithmetic operators).

## Usage

```console
$ ./pivot_table -h
Usage ./pivot_table [OPTIONS] [FILES]

 -e [EMPTY]     value for Empty cell(default: "")
 -d [DELIM]     Delimiter(default: TAB)
 -r [ROW]       Row filed index
 -c [COL]       Col filed index
 -v [VAL]       Val filed index
 -i [IN_HEADER] # of Input header lines (default: 0)
 -o             Output header
```

## Sample

```console
$ # create sample data
$ for r in `seq -w 10`; do
    for c in `seq -w 20`; do
      printf "$r\t$c\tv$r$c\n";
    done;
  done | sort -R  | head  -n 150 > sample.tsv
$ head sample.tsv  # row col value
08	08	v0808
01	17	v0117
04	05	v0405
07	18	v0718
04	04	v0404
02	14	v0214
06	02	v0602
10	10	v1010
06	19	v0619
03	07	v0307
$ ./pivot_table -r 1 -c 1 -v 3 -e vXXXX -o sample.tsv
	01	02	03	04	05	06	07	08	09	10
01	v0101	v0112	vXXXX	v0205	v0219	vXXXX	v0313	vXXXX	v0406	v0416	vXXXX	v0506	vXXXX	v0604	v0616	vXXXX	v0710	vXXXX	v0806	vXXXX	v0904	v0916	vXXXX	v1011
02	v0102	v0114	vXXXX	v0206	vXXXX	v0301	v0314	vXXXX	v0407	v0417	vXXXX	v0507	vXXXX	v0605	v0618	vXXXX	v0711	vXXXX	v0808	vXXXX	v0905	v0917	vXXXX	v1012
03	v0103	v0117	vXXXX	v0207	vXXXX	v0302	v0316	vXXXX	v0408	v0418	vXXXX	v0509	vXXXX	v0607	v0619	vXXXX	v0712	vXXXX	v0809	vXXXX	v0906	v0918	vXXXX	v1013
04	v0104	v0118	vXXXX	v0208	vXXXX	v0304	v0317	vXXXX	v0409	v0419	vXXXX	v0510	vXXXX	v0608	v0620	vXXXX	v0714	vXXXX	v0811	vXXXX	v0907	v0919	vXXXX	v1014
05	v0105	v0119	vXXXX	v0209	vXXXX	v0305	v0319	vXXXX	v0410	v0420	vXXXX	v0511	vXXXX	v0610	vXXXX	v0702	v0715	vXXXX	v0814	vXXXX	v0908	vXXXX	v1001	v1015
06	v0107	v0120	vXXXX	v0211	vXXXX	v0306	v0320	vXXXX	v0411	vXXXX	v0501	v0514	vXXXX	v0611	vXXXX	v0703	v0716	vXXXX	v0815	vXXXX	v0909	vXXXX	v1002	v1016
07	v0108	vXXXX	v0201	v0214	vXXXX	v0307	vXXXX	v0401	v0412	vXXXX	v0502	v0515	vXXXX	v0612	vXXXX	v0705	v0717	vXXXX	v0816	vXXXX	v0912	vXXXX	v1003	v1017
08	v0109	vXXXX	v0202	v0215	vXXXX	v0308	vXXXX	v0403	v0413	vXXXX	v0503	v0519	vXXXX	v0613	vXXXX	v0706	v0718	vXXXX	v0817	vXXXX	v0913	vXXXX	v1007	v1018
09	v0110	vXXXX	v0203	v0217	vXXXX	v0309	vXXXX	v0404	v0414	vXXXX	v0504	vXXXX	v0601	v0614	vXXXX	v0707	v0720	vXXXX	vXXXX	v0902	v0914	vXXXX	v1009	v1019
10	v0111	vXXXX	v0204	v0218	vXXXX	v0310	vXXXX	v0405	v0415	vXXXX	v0505	vXXXX	v0602	v0615	vXXXX	v0709	vXXXX	v0802	vXXXX	v0903	v0915	vXXXX	v1010	v1020
```

# rxargs: recursive xargs(1)

Execute given command to each files and re-execute the command to the result recursively.

## Usage

```console
$ ./rxargs -h
Usage ./rxargs [OPTIONS] COMMAND

recursive xargs

 -t [TEMPDIR]   Temporary working directory(default: create by mktemp)
 -n [NUM]       max Number of arguments(default: auto by xargs)
 -x             Execute COMMAND and print output file name
```

## Sample

```console
$ for i in `seq 8`; do
    echo $i > file${i};
  done
$ # combine files by paste(1) in groupt by 3
$ echo file* | ./rxargs -n3 -- paste -d,
1,2,3,4,5,6,7,8
$ # is same as (with bash extension)
$ paste -d, \
  <(paste -d, file1 file2 file3) \
  <(paste -d, file4 file5 file6) \
  <(paste -d, file7 file8)
1,2,3,4,5,6,7,8
```

## why this command is useful?

xargs(1) can invoke the command for each N inputs, but the outputs will join vertically.
But sometime/someone want join the outputs horizontally.
In such a case, rxargs with paste(1) can be use.

In general, rxargs can join huge amount of input by some command which have an associative property.

