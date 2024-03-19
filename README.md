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

pub fn insert_in_begin(data: String, curr_head: Option(Node)) -> NodesReturned {
    case curr_head {
      None -> #(None, Node(data, None, None))
      Some(hd) -> {
        let updated_head: Option(Node) = Some(Node(hd.data, Some(Node(data, None, Some(hd))), hd.next))
        let node = Node(data, None, updated_head)
        #(updated_head, node)
      }
    }
}
```
