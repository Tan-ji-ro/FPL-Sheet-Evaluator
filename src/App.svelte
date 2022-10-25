<script>
  import fpl from "./assets/logo-fpt-polytechnic-inkythuatso.svg";
  import Counter from "./lib/Counter.svelte";
  import Importer from "./lib/Importer.svelte";

  import {
    Styles,
    Navbar,
    NavbarBrand,
    Nav,
    NavItem,
    NavLink,
    Col,
    Container,
    Row,
  } from "sveltestrap";
</script>

<main>
  <Styles />

  <div>
    <Navbar color="light" light expand="sm">
      <NavbarBrand href="https://caodang.fpt.edu.vn/" target="_blank">
        <img src={fpl} class="logo fpl" alt="FPL Logo" />
      </NavbarBrand>
      <Nav navbar>
        <NavItem>
          <NavLink href="#components/">Hướng dẫn</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="https://caodang.fpt.edu.vn/fpt-education">Thông tin</NavLink
          >
        </NavItem>
      </Nav>
    </Navbar>
  </div>

  <h1>Hệ thống kiểm tra bảng Fpoly</h1>

  <Container>
    <Row>
      <Col>
        <div class="card">
          <h3>Nhập, kiểm tra và tính toán</h3>
          <p class="info">
            Nhập bảng điểm vào hệ thống và thực hiện kiểm tra, tính toán và xuất bản thông tin.
          </p>
          <Importer />
        </div>
      </Col>
      <Col>
        <div class="card">
          <h3>How to use</h3>
          <p class="info">
            Nhập bảng điểm dưới đuôi tệp XLSX và nhấn "Tải lên và kiểm tra" để kiểm tra ô trống.
            (Chỉ sử đụng được chức năng trên cho 1 số bảng.)
          </p>
          <p class="info">
            Khi bạn đã kiểm tra xong, nếu bạn nhập đầy đủ số điểm, bạn có thể yêu cầu hệ thống tính toán và xuất kết quả.
            Chỉ cần bạn nhấn "Tải lên, tính toán và xuất bản" và chờ kết quả cuối cùng. 
            (Chỉ sử đụng được chức năng trên cho 1 số bảng.)
          </p>
          <p class="info">
            Cảnh báo: Kết quả tính toán trong hệ thống sử dụng mã nhị phân cho nên sẽ không chính xác.
            Bạn nên tính toán lại bằng hệ thập phân để nâng cao độ chính xác.
            Không nên nhấn "Tải lên, tính toán và xuất bản" khi trong bảng có giá trị trống. 
            Hệ thống sẽ gặp lỗi tính toán, cho nên trong bảng phải có đủ giá trị mới có thể tính toán được.
          </p>
          <Counter />
        </div>
      </Col>
    </Row>
  </Container>

  <p class="info">
    Trang web cho phép bạn kiểm tra tính toán và xem kết quả trong bảng XLSX.
  </p>

  <p class="read-the-docs">Bản quyền thuộc về FPT Polytechnic</p>
</main>

<style>
  .logo {
    height: 8em;
    padding: 0.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.fpl:hover {
    filter: drop-shadow(0 0 2em #f56e23);
  }
  .info {
    padding-top: 12px;
    padding-bottom: 12px;
  }
  .read-the-docs {
    padding-top: 6px;
    padding-bottom: 6px;
    color: #888;
  }
</style>
