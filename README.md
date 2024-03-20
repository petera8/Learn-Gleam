```gleam
import gleam/io
import gleam/option.{type Option, None, Some}

pub type Node {
  Node(value: String, prev: Option(Node), next: Option(Node))
}

pub fn create_node(node_name: String) -> Node {
  Node(node_name, None, None)
}

pub fn append(node new_node: Node, tail old_tail: Node) -> Node {
  case old_tail.next{
    None -> Node(old_tail.value, old_tail.prev, Some(new_node))
    Some(next) -> Node(old_tail.value, old_tail.prev, Some(append(new_node, next)))
  }
}

pub fn prepend(node new_node: Node, head old_head: Node){
  case old_head.prev {
    None -> Node(old_head.value, Some(new_node), old_head.next)
    Some(prev) -> Node(old_head.value, Some(prepend(new_node, prev)), old_head.next)
  }
}

pub fn main() {
  let node = append(node: create_node("Node B"), tail: create_node("Node A"))
  let node2 = prepend(node: create_node("Node C"), head: node)
  let node3 = prepend(node: create_node("Node D"), head: node2)
  let node4 = append(node: create_node("Node E"), tail: node3)
  io.debug(node4)
}


// OUTPUT:
Node(
  value: "Node A",
  prev: Some(
          Node(
            value: "Node C",
            prev: Some(
                    Node(
                      value: "Node D",
                      prev: None,
                      next: None)
            ),
            next: None)
  ),
  next: Some(
          Node(
              value: "Node B",
              prev: None,
              next: Some(
                      Node(value: "Node E", prev: None, next: None)
              )
          )
  )
)
```
