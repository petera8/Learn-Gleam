import gleam/io
import gleam/option.{type Option, None, Some}

pub type Node {
  Node(data: String, prev: Option(Node), next: Option(Node))
}

pub fn main() {
  let test_1 = insertinbegin("T1", None)
  let test_2 = insertinbegin("T2", test_1)
  io.debug(test_2)
}

pub fn insertinbegin(data: String, curr_head: Option(Node)) -> Option(Node) {
    case curr_head {
      None -> Some(Node(data, None, None))
      Some(hd) -> {
        let updated_head: Option(Node) = Some(Node(hd.data, Some(Node(data, None, Some(hd))), hd.next))
        let node = Node(data, None, updated_head)
        Some(node)
      }
    }
}
