```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;

class TrieNode {
    String word;
    HashMap<Character, TrieNode> children;
    public TrieNode() {
        word = null;
        children = new HashMap<Character, TrieNode>();
    }
};


class TrieTree{
    TrieNode root;

    public TrieTree(TrieNode TrieNode) {
        root = TrieNode;
    }

    public void insert(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            if (!node.children.containsKey(word.charAt(i))) {
                node.children.put(word.charAt(i), new TrieNode());
            }
            node = node.children.get(word.charAt(i));
        }
        node.word = word;
    }
}

public class Eigen {
    private int[] dx = {1, 0, -1, 0};
    private int[] dy = {0, 1, 0, -1};

    private void search(char[][] board,
                       int x,
                       int y,
                       TrieNode root,
                       List<String> results) {
        if (!root.children.containsKey(board[x][y])) {
            return;
        }

        TrieNode child = root.children.get(board[x][y]);

        if (child.word != null) {
            if (!results.contains(child.word)) {
                results.add(child.word);
            }
        }

        char tmp = board[x][y];
        board[x][y] = 0;  

        for (int i = 0; i < 4; i++) {
            if (!isValid(board, x + dx[i], y + dy[i])) {
                continue;
            }
            search(board, x + dx[i], y + dy[i], child, results);
        }

        board[x][y] = tmp; 
    }

    private boolean isValid(char[][] board, int x, int y) {
        if (x < 0 || x >= board.length || y < 0 || y >= board[0].length) {
            return false;
        }

        return board[x][y] != 0;
    }

    private List<String> helper(List<String> words, char[][] board) {
        List<String> results = new ArrayList<String>();

        TrieTree tree = new TrieTree(new TrieNode());
        for (String word : words){
            tree.insert(word);
        }

        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                search(board, i, j, tree.root, results);
            }
        }

        return results;
    }
    public List<Integer> find_integers(ArrayList<Integer> numbers, ArrayList<List<Integer>> matrix) {
        ArrayList<String> words = new ArrayList<>();
        for (Integer num : numbers){
            words.add(num + "");
        }
        int m = matrix.size();
        int n = matrix.get(0).size();
        char[][] strMat = new char[m][n];
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                strMat[i][j] = (char) (matrix.get(i).get(j) + '0');
            }
        }
        List<String> tmp = helper(words, strMat);
        List<Integer> res = new ArrayList<>();
        for (String str : tmp){
            res.add(Integer.parseInt(str));
        }
        return res;
    }
    /** test:
    public static void main(String[] args){
        ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(123, 895, 119, 1037));
        ArrayList<List<Integer>> matrix = new ArrayList<>();
        matrix.add(Arrays.asList(1, 2, 3, 4));
        matrix.add(Arrays.asList(3, 5, 9, 8));
        matrix.add(Arrays.asList(8, 0, 3, 7));
        matrix.add(Arrays.asList(6, 1, 9, 2));
        List<Integer> res = new Eigen().find_integers(numbers, matrix);
        for (Integer element : res){
            System.out.print(element + " ");
        }
    }
    */

}
```