<template>
  <div>
    <Navbar
      @visualize-dijktras="visualizeDijkstra"
      @clear-board="clearBoard"
    ></Navbar>
  </div>
  <div class="grids">
    <div v-for="(array, i) in grid" :key="i">
      <div v-for="(obj, j) in array" :key="j">
        <node
          :x="i"
          :y="j"
          :obj="obj"
          @mousedown="!running ? handleMousedown(i, j):null"
          @mouseup="!running ? handleMouseup(i, j):null"
          @mouseenter="!running ? handleMouseEnter(i, j):null"
          :class="{ wall: obj.isWall }"
        ></node>
      </div>
    </div>
  </div>
</template>

<script>
import Node from "./Node.vue";
import Navbar from "./Navbar.vue";

export default {
  name: "PathfindingVisualizer",
  components: {
    Node,
    Navbar,
  },
  props: {},
  data() {
    return {
      // grid containing array of objects which represents nodes
      grid: [],
      initialGrid: [],
      visitedNodesInOrder: [],
      START_NODE_ROW: 10,
      START_NODE_COL: 10,
      FINISH_NODE_ROW: 10,
      FINISH_NODE_COL: 35,
      //   node object properties
      col: null,
      row: null,
      isStart: null,
      isFinish: null,
      distance: Infinity,
      isVisited: false,
      isWall: false,
      previousNode: null,
      extraClassName: null,
      //   end here
      mouseIsPresses: false,
      node: null,
      running: false,
    };
  },
  methods: {
    //   creating grids and pushing default node objects into grid array
    getInitialgrid() {
      for (let row = 0; row < 27; row++) {
        const currentRow = [];
        for (let col = 0; col < 60; col++) {
          currentRow.push(this.createNode(row, col));
        }
        this.grid.push(currentRow);
        this.initialGrid.push(currentRow);
      }
    },
    createNode(row, col) {
      this.row = row;
      this.col = col;
      this.isStart = row === this.START_NODE_ROW && col === this.START_NODE_COL;
      this.isFinish =
        row === this.FINISH_NODE_ROW && col === this.FINISH_NODE_COL;
      if (this.isStart) {
        this.extraClassName = "start-node";
      } else if (this.isFinish) {
        this.extraClassName = "finish-node";
      } else {
        this.extraClassName = "";
      }

      // each node have these properties
      return {
        row: this.row,
        col: this.col,
        isStart: this.isStart,
        isFinish: this.isFinish,
        distance: this.distance,
        isVisited: this.isVisited,
        isWall: this.isWall,
        previousNode: this.previousNode,
        extraClassName: this.extraClassName,
      };
    },
    visualizeDijkstra() {
      this.running = true;
      const startNode = this.grid[this.START_NODE_ROW][this.START_NODE_COL];
      const finishNode = this.grid[this.FINISH_NODE_ROW][this.FINISH_NODE_COL];
      const grid = this.grid;
      this.visitedNodesInOrder = this.dijkstra(grid, startNode, finishNode);
      const nodesInShortestPathOrder = this.getNodesInShortestPathOrder(
        finishNode
      );
      this.animateDijkstra(this.visitedNodesInOrder, nodesInShortestPathOrder);
    },
    dijkstra(grid, startNode, finishNode) {
      this.visitedNodesInOrder = [];
      startNode.distance = 0;
      const unvisitedNodes = this.getAllNodes(grid);
      while (unvisitedNodes.length) {
        this.sortNodesByDistance(unvisitedNodes);
        const closestNode = unvisitedNodes.shift();
        if (closestNode.isWall) continue;
        if (closestNode.distance === Infinity) return this.visitedNodesInOrder;
        closestNode.isVisited = true;
        this.visitedNodesInOrder.push(closestNode);
        if (closestNode === finishNode) return this.visitedNodesInOrder;
        this.updateUnvisitedNeighbors(closestNode, grid);
      }
    },
    getAllNodes(grid) {
      const nodes = [];
      for (const row of grid) {
        for (const node of row) {
          nodes.push(node);
        }
      }
      return nodes;
    },
    sortNodesByDistance(unvisitedNodes) {
      unvisitedNodes.sort((nodeA, nodeB) => nodeA.distance - nodeB.distance);
    },
    updateUnvisitedNeighbors(node, grid) {
      const unvisitedNeighbors = this.getUnvisitedNeighbors(node, grid);
      for (const neighbor of unvisitedNeighbors) {
        neighbor.distance = node.distance + 1;
        neighbor.previousNode = node;
      }
    },
    getUnvisitedNeighbors(node, grid) {
      const neighbors = [];
      const { col, row } = node;
      if (row > 0) neighbors.push(grid[row - 1][col]);
      if (row < grid.length - 1) neighbors.push(grid[row + 1][col]);
      if (col > 0) neighbors.push(grid[row][col - 1]);
      if (col < grid[0].length - 1) neighbors.push(grid[row][col + 1]);
      return neighbors.filter((neighbor) => !neighbor.isVisited);
    },
    getNodesInShortestPathOrder(finishNode) {
      const nodesInShortestPathOrder = [];
      let currentNode = finishNode;
      while (currentNode !== null) {
        nodesInShortestPathOrder.unshift(currentNode);
        currentNode = currentNode.previousNode;
      }
      return nodesInShortestPathOrder;
    },
    animateDijkstra(visitedNodesInOrder, nodesInShortestPathOrder) {
      for (let i = 1; i <= this.visitedNodesInOrder.length; i++) {
        if (i === visitedNodesInOrder.length) {
          setTimeout(() => {
            this.animateShortestPath(nodesInShortestPathOrder);
          }, 10 * i);
          return;
        }
        setTimeout(() => {
          const node = visitedNodesInOrder[i];
          if (
            document.getElementById(`node-${node.row}-${node.col}`).className !=
            "node finish-node"
          ) {
            document.getElementById(`node-${node.row}-${node.col}`).className =
              "node node-visited";
          }
        }, 10 * i);
      }
    },
    animateShortestPath(nodesInShortestPathOrder) {
      for (let i = 0; i < nodesInShortestPathOrder.length; i++) {
        setTimeout(() => {
          const node = nodesInShortestPathOrder[i];
          document.getElementById(`node-${node.row}-${node.col}`).classList.add("node-shortest-path");
        }, 50 * i);
      }
      this.running = false;
    },
    handleMousedown(row, col) {
      const node = this.grid[row][col];
      if (node.isStart) {
        this.node = "start";
        node.extraClassName = null;
      } else if (node.isFinish) {
        this.node = "finish";
        node.extraClassName = null;
      }
      // if (node.isStart || node.isFinish) return;
      else {
        var grid = this.grid;
        const newGrid = this.getNewGridWithWallToggled(grid, row, col);
        this.grid=newGrid;
        this.mouseIsPresses = true;
      }
    },
    handleMouseup(row, col) {
      if (this.mouseIsPresses) {
        this.mouseIsPresses = false;
      } else {
        if (this.node === "start") {
          const nod = this.grid[row][col];
          this.START_NODE_ROW = row;
          this.START_NODE_COL = col;
          nod.isStart = true;
          nod.extraClassName = "start-node";
        } else if (this.node === "finish") {
          const nod = this.grid[row][col];
          this.FINISH_NODE_ROW = row;
          this.FINISH_NODE_COL = col;
          nod.isFinish = true;
          nod.extraClassName = "finish-node";
        }
      }
    },
    handleMouseEnter(row, col) {
      if (!this.mouseIsPresses) return;
      const node = this.grid[row][col];
      if (node.isStart || node.isFinish) return;
      var grid = this.grid;
      const newGrid = this.getNewGridWithWallToggled(grid, row, col);
      this.grid = newGrid;
    },
    getNewGridWithWallToggled(grid, row, col) {
      const newGrid = grid.slice();
      const node = grid[row][col];
      const newNode = {
        ...node,
        isWall: !node.isWall,
      };
      newGrid[row][col] = newNode;
      return newGrid;
    },
    clearBoard() {
      // re render the component to clear the board
      if (!this.running) {
        this.$emit("clear");
      }
    },
  },
  created() {
    document.title = "pathfinding visualizer";
    this.getInitialgrid();
  },
};
</script>

<style scoped>
.grids {
  margin: 100px 0 0 6px;
}
</style>
