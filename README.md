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
    None -> Node(new_node.value, Some(Node(old_tail.value, old_tail.prev, Some(new_node))), None)
    Some(next) -> Node(old_tail.value, old_tail.prev, Some(append(new_node, next)))
  }
}

pub fn prepend(node new_node: Node, head old_head: Node){
  case old_head.prev {
    None -> Node(new_node.value, new_node.prev, Some(Node(old_head.value, Some(new_node), old_head.next)))
    Some(prev) -> Node(old_head.value, Some(prepend(new_node, prev)), old_head.next)
  }
}

pub fn get_first(node node: Node) -> Node {
  case node.prev {
    None -> node
    Some(prev) -> get_first(prev)
  }  
}

pub fn get_last(node node: Node) -> Node {
  case node.next {
    None -> node
    Some(next) -> get_last(next)
  }  
}

pub fn main() {
  // Order: D <-> C <-> A <-> B <-> E
  let d_list = append(node: create_node("Node B"), tail: create_node("Node A"))
   |> prepend(node: create_node("Node C"), head: _)
   |> prepend(node: create_node("Node D"), head: _)
   |> append(node: create_node("Node E"), tail: _)
   
  let first_node = get_first(node: d_list)
  io.debug(first_node)
  io.println("")
  let last_node = get_last(node: d_list)
  io.debug(last_node)
}
```
