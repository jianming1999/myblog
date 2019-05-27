二叉树的实现
```javascript
// 二叉树类
function BinaryTree(){
  节点类
	function TreeNode(key){
		this.key = key;
		this.left = null;
		this.right = null;
	}
	var root = null;
  // 递归插入节点
	function insertNode(node, newNode){
		if(newNode.key < node.key ){
			if(node.left === null){
				node.left = newNode;
			}else{
				insertNode(node.left, newNode);
			}
		}else{
			if(node.right === null){
				node.right = newNode;
			}else{
				insertNode(node.right, newNode);
			}
		}
	};
  // 向二叉树添加节点
	this.insert = function(key){
		var newNode = new TreeNode(key);
		if(root === null){
			root = newNode;
		}else{
			insertNode(root, newNode);
		}
	}
  // 获取根节点
	this.getRoot = function(){
		return root;
	}
}
// 测试
var btree = new BinaryTree();
var arr = [8, 1, 9 ,10, 11, 3, 6, 20, 12];
arr.forEach(function(item){
	btree.insert(item);
});
console.log(btree.getRoot());
```
