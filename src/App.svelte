<script>
  import fpl from "./assets/logo-fpt-polytechnic-inkythuatso.svg";
  import Counter from "./lib/Counter.svelte";
  import Importer from "./lib/Importer.svelte";
  import TableExample from "./lib/TableExample.svelte";

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
          <h3>Cách sử dụng</h3>
          <p class="info-p">
            Nhập bảng điểm dưới đuôi tệp XLSX và nhấn "Tải lên và kiểm tra" để kiểm tra ô trống.
            (Chỉ sử đụng được chức năng trên cho 1 số bảng.)
          </p>
          <p class="info-p">
            Khi bạn đã kiểm tra xong, nếu bạn nhập đầy đủ số điểm, bạn có thể yêu cầu hệ thống tính toán và xuất kết quả.
            Chỉ cần bạn nhấn "Tải lên, tính toán và xuất bản" và chờ kết quả cuối cùng. 
            (Chỉ sử đụng được chức năng trên cho 1 số bảng.)
          </p>
          <TableExample />
          <h4 class="info-t">Cảnh báo khi sử dụng tiện ích trên:</h4>
          <ol class="info-l">
            <li>
              Kết quả tính toán trong hệ thống sử dụng mã nhị phân cho nên sẽ
              không chính xác. Bạn nên tính toán lại bằng hệ thập phân để nâng
              cao độ chính xác.
            </li>
            <li>
              Nếu thông tin trong bảng còn thiếu, hệ thống sẽ tiếp tục vô hiệu hóa
              nút "Tải lên, tính toán và xuất bản" cho đến khi toàn bộ thông tin đã
              được ghi đầy đủ để đảm bảo an toàn cho chức năng tính toán. 
            </li>
          </ol>
          <Counter />
        </div>
      </Col>
    </Row>
  </Container>

  <p class="info">
    Trang web cho phép bạn kiểm tra tính toán và xem kết quả trong bảng XLSX.
  </p>

  <p class="read-the-docs">Bản quyền thuộc về FPT Polytechnic</p>
  
  <p class="read-the-docs">Framework JavaScript được sử dụng: Svelte v3</p>

  <p class="read-the-docs">
    Plugin được sử dụng: read-excel-file bởi Nikolay Kuchumov, ExcelJS bởi
    Guyonroche, file-saver bởi eligrey, ChartJS, Svelte-ChartJS bởi Saurav Kanchan
  </p>
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
  .info-p {
    padding-top: 10px;
    padding-bottom: 10px;
    text-align: justify;
    text-justify: inter-word;
  }
  .info-t{
    padding-top: 5px;
    padding-bottom: 5px;
    text-align: justify;
    text-justify: inter-character;
  }
  .info-l {
    padding-top: 8px;
    padding-bottom: 8px;
    text-align: justify;
    text-justify: inter-word;
  }
  .read-the-docs {
    padding-top: 6px;
    padding-bottom: 6px;
    color: #888;
  }
</style>
