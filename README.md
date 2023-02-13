# 프로젝트 설명
+ 테이블 뷰 컨트롤러(Table View Controller)는 데이터를 목록 형태로 보여주기 위한 가장 좋은 방법으로 사용자에게 목록 형태의 정보를 제공해 줄 뿐만 아니라 목록의 특정 항목을 선택하여 세부 사항을 표시할 때 유용합니다. 이런 테이블 뷰 컨트롤러(Table View Controller)의 대표적인 앱에는 알람, 메일, 연락처 등이 있습니다.
---

# 전체 소스 & 해석
+ TableViewController.swift
```swift

import UIKit

// items의 내용
var items = ["책 구매", "철수와 약속", "스터디 준비하기"]

// ImageFile 이름
var itemsImageFile = ["cart.png", "clock.png", "pencil.jpeg"]

 

class TableViewController: UITableViewController {

 

    @IBOutlet var tvListView: UITableView!

    override func viewDidLoad() {

        super.viewDidLoad()

 

        // Uncomment the following line to preserve selection between presentations

        // self.clearsSelectionOnViewWillAppear = false

 

        // Uncomment the following line to display an Edit button in the navigation bar for this view controller.

         self.navigationItem.leftBarButtonItem = self.editButtonItem

    }

// 뷰가 노출될 때마다 리스트의 데이터를 다시 불러오기
    override func viewWillAppear(_ animated: Bool) {

        tvListView.reloadData()

    }

 

    // MARK: - Table view data source

 
// 테이블 안의 섹션 개수 1로 설정
    override func numberOfSections(in tableView: UITableView) -> Int {

        // #warning Incomplete implementation, return the number of sections

        return 1

    }

 
//섹션 당 열의 개수 전달
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {

        // #warning Incomplete implementation, return the number of rows

        return items.count

    }

 

    
// items와 itemsImageFile 값을 셀에 삽입
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

        let cell = tableView.dequeueReusableCell(withIdentifier: "myCell", for: indexPath)

        

        cell.textLabel?.text = items[ (indexPath as NSIndexPath).row]

        cell.imageView?.image = UIImage(named: itemsImageFile[(indexPath as NSIndexPath).row])

 

        return cell

    }

    

 

    /*
    
   
    // Override to support conditional editing of the table view.

    override func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {

        // Return false if you do not want the specified item to be editable.

        return true

    }

    */

 

    
// 목록 삭제
    // Override to support editing the table view.

    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {

        if editingStyle == .delete {

            // Delete the row from the data source

            items.remove(at: (indexPath as NSIndexPath).row)

            itemsImageFile.remove(at: (indexPath as NSIndexPath).row)

            tableView.deleteRows(at: [indexPath], with: .fade)

        } else if editingStyle == .insert {

            // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view

        }    

    }

    

    

    override func tableView(_ tableView: UITableView, titleForDeleteConfirmationButtonForRowAt indexpath: IndexPath) ->String? {

        return "삭제"

    }

    

    
// 목록 순서 바꾸기 
    // Override to support rearranging the table view.

    override func tableView(_ tableView: UITableView, moveRowAt fromIndexPath: IndexPath, to: IndexPath) {

        let itemToMove = items[(fromIndexPath as NSIndexPath).row]

        let itemImageToMove = itemsImageFile[(fromIndexPath as NSIndexPath).row]

        items.remove(at: (fromIndexPath as NSIndexPath).row)

        itemsImageFile.remove(at: (fromIndexPath as NSIndexPath).row)

        items.insert(itemToMove, at: (to as NSIndexPath).row)

        itemsImageFile.insert(itemImageToMove, at: (to as NSIndexPath).row)

 

    }

   

 

    /*

    // Override to support conditional rearranging of the table view.

    override func tableView(_ tableView: UITableView, canMoveRowAt indexPath: IndexPath) -> Bool {

        // Return false if you do not want the item to be re-orderable.

        return true

    }

    */

 

    

    // MARK: - Navigation

 

    // In a storyboard-based application, you will often want to do a little preparation before navigation

//세그웨이를 이용하여 디테일 뷰로 전환
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {

        // Get the new view controller using segue.destination.

        // Pass the selected object to the new view controller.

        if segue.identifier == "sgDetail" {

            let cell = sender as! UITableViewCell

            let indexpath = self.tvListView.indexPath(for: cell)

            let detailView = segue.destination as! DetailViewController

            detailView.receiveItem(items[((indexpath! as NSIndexPath).row)])

    }

    

}

}

```

---

+ AddViewController.swift
```swift

import UIKit

 

class AddViewController: UIViewController {

 

    @IBOutlet var tfAddItem: UITextField!

    override func viewDidLoad() {

        super.viewDidLoad()

 

        // Do any additional setup after loading the view.

    }

    
// 새 목록 추가
    @IBAction func btnAddItem(_ sender: UIButton) {

        items.append(tfAddItem.text!)

        itemsImageFile.append("clock.png")

        tfAddItem.text=""

        _ = navigationController?.popViewController(animated: true)

    }

    

    /*

    // MARK: - Navigation

 

    // In a storyboard-based application, you will often want to do a little preparation before navigation

    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {

        // Get the new view controller using segue.destination.

        // Pass the selected object to the new view controller.

    }

    */

 

}
```

---
+ DetailViewController.swift
```swift

import UIKit

 

class DetailViewController: UIViewController {

    

    var receiveItem = ""

 

    @IBOutlet var lblItem: UILabel!

    override func viewDidLoad() {

        super.viewDidLoad()

 

        // Do any additional setup after loading the view.

        lblItem.text = receiveItem

    }

    
// Main View에서 변수 받아오기 
    func receiveItem(_ item: String)

    {

        receiveItem = item

    }

    

 

    /*

    // MARK: - Navigation

 

    // In a storyboard-based application, you will often want to do a little preparation before navigation

    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {

        // Get the new view controller using segue.destination.

        // Pass the selected object to the new view controller.

    }

    */

 

}
```
---
# 스크린샷
![스크린샷 2022-05-13 오후 4 36 05](https://user-images.githubusercontent.com/106370789/173593506-8ee64433-9636-4c8a-82b2-c70a215a0e61.png)
![스크린샷 2022-05-13 오후 4 36 16](https://user-images.githubusercontent.com/106370789/173594289-762b9475-2fb0-42af-8769-8c60a6ca51ea.png)


![스크린샷 2022-05-13 오후 4 36 29](https://user-images.githubusercontent.com/106370789/173594383-15e034d0-0d0b-442c-a754-693898b7e806.png)
![스크린샷 2022-05-13 오후 4 36 44](https://user-images.githubusercontent.com/106370789/173594456-0c8a781e-9acf-4a7a-931b-24520875c57a.png)

---
# 출처
+ 스위프트로 아이폰 앱 만들기 입문 개정 6판





