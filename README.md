# 3D_Packing
### 问题描述：
物流公司在流通过程中，需要将打包完毕的箱子装入到一个货车的车厢中，为了提高物流效率，需要将车厢尽量填满，显然，车厢如果能被100%填满是最优的，但通常认为，车厢能够填满85%，可认为装箱是比较优化的。
设车厢为长方形，其长宽高分别为L，W，H；共有n个箱子，箱子也为长方形，第i个箱子的长宽高为li，wi，hi（n个箱子的体积总和是要远远大于车厢的体积），做以下假设和要求：
1. 长方形的车厢共有8个角，并设靠近驾驶室并位于下端的一个角的坐标为（0,0,0），车厢共6个面，其中长的4个面，以及靠近驾驶室的面是封闭的，只有一个面是开着的，用于工人搬运箱子；
2. 需要计算出每个箱子在车厢中的坐标，即每个箱子摆放后，其和车厢坐标为（0,0,0）的角相对应的角在车厢中的坐标，并计算车厢的填充率。
### 启发式算法：
启发式方法是人们在解决问题时所采取的一种根据经验规则来处理问题的方法。其特点是在解决问题时，利用过去的经验，选择已经行之有效的方法，而不是系统地、按照确定的步骤去寻求答案。但由于这种方法具有试错的特点，所以也有失败的可能。
启发式方法是一种非常高效的解决问题方法，对于NP完全类的组合优化问题，目前缺少一般的算法来求解，精确算法又太耗时，所以启发式算法被广泛应用到这些问题的求解中。对于装箱问题亦是如此，自从该问题被提出以来，已经有很多启发式算法应用其中。
具体代码：
```
vector<Block> GenSimpleBlock(Space container, vector<Box> box, vector<int> num) {
	vector<Block> blockTable;
	for (Box b : box) {
		for (int nx = 1; nx <= num[b.type]; nx++) {
			for (int ny = 1; ny <= num[b.type] / nx; ny++) {
				for (int nz = 1; nz <= num[b.type] / (nx * ny); nz++) {
					if (b.lx * nx <= container.lx && b.ly * ny <= container.ly && b.lz * nz <= container.lz) {
						vector<int> require(num.size());
						require[b.type] = nx * ny * nz;

						Block block;
						block.lx = b.lx * nx;
						block.ly = b.ly * ny;
						block.lz = b.lz * nz;
						block.require = require;
						block.volumn = block.lx * block.ly * block.lz;
						block.times = 0;
						blockTable.push_back(block);
					}
				}
			}
		}
	}
	sort(blockTable.begin(), blockTable.end(), BlockCmp);
	return blockTable;
}
```
