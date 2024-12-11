I have an amd r5 5600X with 6 cores so the tests are up to 6 threads.
Question 1:
naive-native:
	1 thread:
	0.147311+0.14714+0.150451+0.152551+0.149961+
	0.150141+0.153021+0.15417+0.153961+0.1527
	avg: 0.1511407
	
	2 threads:
	0.325752+0.287632+0.274081+0.339252+0.304822+
	0.265312+0.413392+0.353162+0.381252+0.368002
	avg: 0.3312659
	
	4 threads:
	0.338732+0.167201+0.441332+0.177321+0.388582+
	0.149561+0.154261+0.255741+0.161561+0.160011
	avg: 0.2394303
	
	6 threads:
	0.207821+0.214751+0.216861+0.199201+0.220161+
	0.210962+0.216601+0.216601+0.225311+0.206821
	avg: 0.2135091
	
As the threads increases the performances gets worst but stabilizes as the amount of threads reaches the number of cores.
I think its because this is a multi threaded processor, two threads are assigned to the same core and slow each other.

Question 2:
(a) 
all-opt-native:
	1 thread:
	0.157701+0.161421+0.1577+0.155081+0.152211
	avg:0.1568228
	
	2 threads:
	0.10844+0.135111+0.135071+0.13788+0.141461
	avg:0.1315926
	
	4 threads:
	0.146631+0.140461+0.144861+0.153981+0.177021
	avg:0.152591
	
	6 threads:
	0.225701+0.228181+0.222521+0.233162+0.215752
	avg:0.2250634
	
I would have expected more improvement but it gets worst as the number of threads increases,
on two threads it the only one that really improves.

(b)
Compared to the same workload on one thread, the speedups/slowdowns are:
	2 threads: X1.19
	4 threads: X1.03
	6 threads: X0.70

Question 3:
(a)
By far the most important optimization is adding padding to the results array, as each thread
had to write on it.

(b)
For each thread their write address are in the same 64B block,  putting them in the same L1 cache line, and due to MESI cache coherence each write on a thread invalidates the copies of that cache line in the other caches. If we put the write address for each thread on a different line, there 
wont be collision and no invalidations.

chunking-native:
	1 thread:
	0.155281+0.163691+0.155891+0.16185+0.162111
	avg:0.1597648
	2 threads:
	0.363202+0.297272+0.350712+0.308552+0.283342
	avg:0.320616
	4 threads:
	0.150921+0.156281+0.307191+0.357682+0.365702
	avg:0.2675554
	6 threads:
	0.217521+0.216571+0.215501+0.213641+0.214112
	avg:0.2154692
res-race-opt-native:
	1 thread:
	0.166891+0.16868+0.154851+0.15876+0.15706
	avg:0.1612484
	2 threads:
	0.379932+0.314892+0.331462+0.326782+0.315912
	avg:0.333796
	4 threads:
	0.355642+0.420942+0.155651+0.15763+0.374692
	avg:0.2929114
	6 threads:
	0.210891+0.204701+0.220221+0.218391+0.231531
	avg:0.217147
chunking-res-race-opt-native:
	1 thread:
	0.171781+0.161701+0.162501+0.158881+0.164351
	avg:0.163843
	2 threads: 
	0.359462+0.330771+0.405882+0.399452+0.325822
	avg:0.3642778
	4 threads:
	0.354182+0.306712+0.423842+0.203291+0.363811
	avg:0.3303676
	6 threads:
	0.218911+0.207981+0.211391+0.222581+0.217361
	avg:0.215645
block-race-opt-native:
	1 thread:
	0.153851+0.157191+0.157371+0.163771+0.159221
	avg:0.158281
	2 threads:
	0.137761+0.10908+0.136441+0.112211+0.152951
	avg:0.1296888
	4 threads:
	0.154471+0.139201+0.138501+0.1446+0.156701
	avg:0.1466948
	6 threads:
	0.217571+0.188331+0.201631+0.196211+0.217401
	avg:0.204229
