<script>
  import { onMount } from "svelte";
  import * as d3 from "d3";

  export let relationships;

  let graphContainer;
  let network;
  let selectedNode = null;

  // Create nodes and links for the network graph
  function createNetworkData() {
    const nodes = [];
    const links = [];

    // Add Batman as the center node
    nodes.push({
      id: "Batman",
      group: "hero",
      value: 10,
    });

    // Add allies
    relationships.allies.forEach((ally) => {
      nodes.push({
        id: ally,
        group: "ally",
        value: 5,
      });

      links.push({
        source: "Batman",
        target: ally,
        value: 1,
        type: "ally",
      });
    });

    // Add enemies
    relationships.enemies.forEach((enemy) => {
      nodes.push({
        id: enemy,
        group: "enemy",
        value: 5,
      });

      links.push({
        source: "Batman",
        target: enemy,
        value: 1,
        type: "enemy",
      });
    });

    return { nodes, links };
  }

  // Create or update the graph
  function renderGraph() {
    const data = createNetworkData();

    // Clear previous graph
    d3.select(graphContainer).selectAll("*").remove();

    const width = graphContainer.clientWidth;
    const height = 400;

    // Create SVG
    const svg = d3
      .select(graphContainer)
      .append("svg")
      .attr("width", width)
      .attr("height", height)
      .attr("viewBox", [0, 0, width, height])
      .style("font", "12px sans-serif");

    // Create color scale
    const color = d3
      .scaleOrdinal()
      .domain(["hero", "ally", "enemy"])
      .range(["#0d6efd", "#198754", "#dc3545"]);

    // Create a force simulation
    const simulation = d3
      .forceSimulation(data.nodes)
      .force(
        "link",
        d3
          .forceLink(data.links)
          .id((d) => d.id)
          .distance(100)
      )
      .force("charge", d3.forceManyBody().strength(-300))
      .force("center", d3.forceCenter(width / 2, height / 2))
      .force(
        "collide",
        d3.forceCollide().radius((d) => Math.sqrt(d.value) * 10 + 10)
      );

    // Create links
    const link = svg
      .append("g")
      .attr("stroke-opacity", 0.6)
      .selectAll("line")
      .data(data.links)
      .join("line")
      .attr("stroke-width", (d) => Math.sqrt(d.value) * 2)
      .attr("stroke", (d) => (d.type === "ally" ? "#198754" : "#dc3545"))
      .attr("stroke-dasharray", (d) => (d.type === "enemy" ? "5,5" : "0"));

    // Create nodes
    const node = svg
      .append("g")
      .selectAll("g")
      .data(data.nodes)
      .join("g")
      .attr("class", "node")
      .call(drag(simulation))
      .on("click", (event, d) => {
        selectedNode = selectedNode === d.id ? null : d.id;
        updateHighlight();
      });

    // Node circles
    node
      .append("circle")
      .attr("r", (d) => Math.sqrt(d.value) * 5 + 5)
      .attr("fill", (d) => color(d.group))
      .attr("stroke", "#fff")
      .attr("stroke-width", 1.5);

    // Node labels
    node
      .append("text")
      .attr("x", 0)
      .attr("y", (d) => -Math.sqrt(d.value) * 5 - 7)
      .attr("text-anchor", "middle")
      .text((d) => d.id)
      .attr("fill", "#343a40")
      .attr("font-weight", (d) => (d.id === "Batman" ? "bold" : "normal"))
      .clone(true)
      .lower()
      .attr("fill", "none")
      .attr("stroke", "white")
      .attr("stroke-width", 3);

    // Node tooltip
    node.append("title").text((d) => d.id);

    // Update positions on each tick of the simulation
    simulation.on("tick", () => {
      link
        .attr("x1", (d) => d.source.x)
        .attr("y1", (d) => d.source.y)
        .attr("x2", (d) => d.target.x)
        .attr("y2", (d) => d.target.y);

      node.attr("transform", (d) => `translate(${d.x},${d.y})`);
    });

    // Function to highlight nodes and links based on selection
    function updateHighlight() {
      if (selectedNode) {
        node.each(function (d) {
          const isSelected = d.id === selectedNode;
          const isConnected = data.links.some(
            (link) =>
              (link.source.id === selectedNode && link.target.id === d.id) ||
              (link.target.id === selectedNode && link.source.id === d.id)
          );

          d3.select(this).style("opacity", isSelected || isConnected ? 1 : 0.2);
        });

        link.each(function (d) {
          const isConnected =
            d.source.id === selectedNode || d.target.id === selectedNode;

          d3.select(this).style("opacity", isConnected ? 1 : 0.1);
        });
      } else {
        // Reset all highlights
        node.style("opacity", 1);
        link.style("opacity", 1);
      }
    }

    // Reset button
    const resetButton = d3.select(".reset-filter");
    if (!resetButton.empty()) {
      resetButton.on("click", () => {
        selectedNode = null;
        updateHighlight();
      });
    }

    // Drag functions
    function drag(simulation) {
      function dragstarted(event) {
        if (!event.active) simulation.alphaTarget(0.3).restart();
        event.subject.fx = event.subject.x;
        event.subject.fy = event.subject.y;
      }

      function dragged(event) {
        event.subject.fx = event.x;
        event.subject.fy = event.y;
      }

      function dragended(event) {
        if (!event.active) simulation.alphaTarget(0);
        event.subject.fx = null;
        event.subject.fy = null;
      }

      return d3
        .drag()
        .on("start", dragstarted)
        .on("drag", dragged)
        .on("end", dragended);
    }
  }

  // Filter nodes by type
  function filterByType(type) {
    const nodes = d3.selectAll(".node");

    if (type === "all") {
      nodes.style("opacity", 1);
    } else {
      nodes.style("opacity", (d) =>
        d.group === type || d.id === "Batman" ? 1 : 0.2
      );
    }
  }

  onMount(() => {
    renderGraph();

    // Handle window resize
    const handleResize = () => {
      renderGraph();
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  });
</script>

<div class="network-analysis">
  <h2>Network Analysis</h2>

  <div class="relationship-header">
    <h3>Relationship Network</h3>

    <div class="filter-controls">
      <button class="filter-button" on:click={() => filterByType("all")}
        >All</button
      >
      <button class="filter-button ally" on:click={() => filterByType("ally")}
        >Allies</button
      >
      <button class="filter-button enemy" on:click={() => filterByType("enemy")}
        >Enemies</button
      >
      <button class="reset-filter" aria-label="Reset filter">
        <i class="icon-refresh">↻</i>
      </button>
    </div>
  </div>

  <div class="filter-info">
    <p>
      Click on a character to see their connections. Drag nodes to rearrange the
      network.
    </p>
  </div>

  <div class="graph-container" bind:this={graphContainer}></div>
</div>

<style>
  .network-analysis {
    background-color: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    padding: 1.5rem;
    margin-bottom: 1rem;
  }

  h2 {
    margin-top: 0;
    font-size: 1.25rem;
    font-weight: 600;
    color: #343a40;
    margin-bottom: 1.5rem;
  }

  .relationship-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;
  }

  h3 {
    font-size: 1rem;
    font-weight: 500;
    color: #343a40;
    margin: 0;
  }

  .filter-controls {
    display: flex;
    gap: 0.5rem;
  }

  .filter-button {
    background: #f8f9fa;
    border: 1px solid #dee2e6;
    color: #495057;
    font-size: 0.875rem;
    padding: 0.25rem 0.5rem;
    border-radius: 4px;
    cursor: pointer;
  }

  .filter-button:hover {
    background: #e9ecef;
  }

  .filter-button.ally {
    border-color: #198754;
    color: #198754;
  }

  .filter-button.enemy {
    border-color: #dc3545;
    color: #dc3545;
  }

  .reset-filter {
    background: none;
    border: none;
    color: #6c757d;
    cursor: pointer;
    padding: 0.25rem;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .reset-filter:hover {
    background-color: #f8f9fa;
  }

  .icon-refresh {
    font-style: normal;
  }

  .filter-info {
    font-size: 0.875rem;
    color: #6c757d;
    margin-bottom: 1rem;
  }

  .filter-info p {
    margin: 0;
  }

  .graph-container {
    width: 100%;
    height: 400px;
    position: relative;
    border: 1px solid #e9ecef;
    border-radius: 4px;
  }
</style>
