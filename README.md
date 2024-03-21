```gleam
import gleam/io
import gleam/list
import gleam/option.{type Option, None, Some}

pub type Node {
  Node(value: String, prev: Option(Node), next: Option(Node))
}

pub fn create_node(node_name: String) -> Node {
  Node(node_name, None, None)
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

pub fn traverse(node: Node) -> List(Node){
  list.concat([traverse_forward(node), traverse_backward(node)])
}

pub fn traverse_forward(node: Node) -> List(Node){
  case node.next{
    None -> [node]
    Some(next) -> list.concat([[next],traverse_forward(node)])
  }
}

pub fn traverse_backward(node: Node) -> List(Node) {
  case node.prev{
    None -> []
    Some(prev) -> {
      list.concat([[node], traverse_backward(prev)])
    }
  }
}

// This function doesn't work
pub fn append(node new_node: Node, tail old_tail: Node) -> Node {
  case old_tail.next{
    None -> Node(old_tail.value, old_tail.prev, Some(Node(new_node.value, Some(old_tail), None)))
    Some(next) -> Node(old_tail.value, old_tail.prev, Some(append(new_node, next)))
  }
}

// This function hasn't been tested
pub fn prepend(node new_node: Node, head old_head: Node){
  case old_head.prev {
    None -> Node(new_node.value, new_node.prev, Some(Node(old_head.value, Some(new_node), old_head.next)))
    Some(prev) -> Node(old_head.value, Some(prepend(new_node, prev)), old_head.next)
  }
}

pub fn main() {
  let d_list = append(node: create_node("Node B"), tail: create_node("Node A"))
  io.debug(d_list)
  io.println("")
  let sec = append(node: create_node("Node C"), tail: d_list)
  io.debug(sec)
}
```
