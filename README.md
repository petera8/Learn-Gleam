```gleam
import gleam/io
import gleam/option.{type Option, None, Some}

pub type Node {
  Node(data: String, prev: Option(Node), next: Option(Node))
}
pub type Head = Option(Node)
pub type NodesReturned = #(Head, Node)

pub fn main() {
  let #(_head, node) = insert_in_begin("Node 1", None)
  let result = insert_in_begin("Node 2", Some(node))
  io.debug(result)
}

pub fn insert_in_begin(data: String, head: Head) -> NodesReturned {
    case head {
      None -> {
        let new_node = Node(data, None, None)
        let new_head = Some(new_node)
        #(new_head, new_node)
      }
      Some(prev_head) -> {
        let updated_head: Head = Some(Node(prev_head.data, Some(Node(data, None, Some(prev_head))), prev_head.next))
        let new_node = Node(data, None, updated_head)
        #(updated_head, new_node)
      }
    }
}
```
