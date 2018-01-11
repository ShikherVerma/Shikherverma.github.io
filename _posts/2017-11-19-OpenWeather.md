---
layout:     post
title:      "Swift. OpenWeather API 사용하기"
subtitle:   "OpenWeather API 사용해서 작은 날씨 APP 을 만들어보자!"
date:       2017-11-19 18:12:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---

### 사용한 API [https://openweathermap.org](https://openweathermap.org) 입니다<br>
---

![screen](/img/posts/OpenWeather.gif)

날씨에 대한 정보를 제공하는 API 를 가지고 간단하게, 현재 위치의 날씨를 UI에 적용 시켜보고, 위도와 경도를 가지고 해당 지역의 날씨를 검색해봅니다.

---

## 현재 위치의 날씨 가져오기. 

![screen](/img/posts/openWeather.jpg) <br>

API 사용법은, [https://openweathermap.org](https://openweathermap.org) 가시면 확인해보실수 있습니다. 

> api.openweathermap.org/data/2.5/weather?`lat=35`&`lon=139`에서 위도와 경도 값을 변경 하면, 그 지역의 날씨 정보를 받을수 있습니다. 
> 
> 네트워크는 데이터 통신은 기본적으로 비동기 처리입니다. 데이터를 가져오는 `시점` 과 사용되어지는 시점, 그리고 UI 변경에 대한 고민을 해주어야 합니다. <br>
> 


```swift
** 날씨값을 반환 하는 데이터 센터 
class WeatherDataManager {
  static var shread: WeatherDataManager = WeatherDataManager()
  let baseURL: String = "https://api.openweathermap.org/data/2.5/weather?"
  let appid: String = "&APPID=646f4d9bc930541a09dcfc5e6eb91c23"
    
	func setCurrentWeather(lati: Float, longi: Float, completion: @escaping Completion)  {
    let strLati = "lat=\(lati)&"
    let strLongi = "lon=\(longi)"  
    let url: URL = URL(string: baseURL+strLati+strLongi+self.appid)!
    var request = URLRequest(url: url)
    request.httpMethod = "GET"
    session.dataTask(with: request) { (data, response, error) in
      let code = (response as! HTTPURLResponse).statusCode
      if let data = data {
          let Arr = try! JSONSerialization.jsonObject(with: data, options: []) as! [String:Any]
        let name = Arr["name"]
        let temp = ((Arr["main"] as! [String:Any])["temp"] as! Float)-273
        let maxTemp = ((Arr["main"] as! [String:Any])["temp_max"] as! Float)-273
        let minTemp = ((Arr["main"] as! [String:Any])["temp_min"] as! Float)-273
        print(minTemp)
        let icon = (Arr["weather"] as! [[String: Any]])[0]["icon"]
        self.returnDic = ["name":name,
                          "temp":temp,
                          "icon":icon,
                          "maxTemp":maxTemp,
                          "minTemp":minTemp]
        print(self.returnDic)
        completion(true, self.returnDic, error)
      }
    }.resume()
  }
}

** MainViewController 부분 
import UIKit
import CoreLocation
class MainViewController: UIViewController {
  @IBOutlet weak var cityLB: UILabel!
  @IBOutlet weak var temperatureLB: UILabel!
  @IBOutlet weak var otherTemp: UILabel!
  @IBOutlet weak var weatherImage: UIImageView!
  @IBOutlet weak var indicate: UIActivityIndicatorView!
  var locationManager: CLLocationManager = CLLocationManager()
  var startLocation: CLLocation!
  let baseURL: String = "https://api.openweathermap.org/data/2.5/weather?"
  let imaURL: String = "https://openweathermap.org/img/w/"
  let lati = "" //lat=35&
  let longi = "" //lon=139"
  
  override func viewDidLoad() {
    super.viewDidLoad()
    let gradient = CAGradientLayer()
    gradient.frame = view.bounds
    gradient.colors = [UIColor(red:0.61, green:0.57, blue:0.77, alpha:0.9).cgColor,
                       UIColor(red:0.45, green:0.44, blue:0.63, alpha:0.92).cgColor,
                       UIColor(red:0.34, green:0.35, blue:0.55, alpha:0.93).cgColor,
                       UIColor(red:0.24, green:0.28, blue:0.47, alpha:1.00).cgColor]
    view.layer.insertSublayer(gradient, at: 0)
    indicate.startAnimating()
    locationManager.desiredAccuracy = kCLLocationAccuracyBest
    locationManager.requestWhenInUseAuthorization()
    locationManager.delegate = self
    locationManager.startUpdatingLocation()
  }
}

extension MainViewController: CLLocationManagerDelegate {
  func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
    guard let locationCoordinate = locations.last?.coordinate else { return }
    locationManager.stopUpdatingLocation()
    let lati = Float(CGFloat(locationCoordinate.latitude))
    let longi = Float(CGFloat(locationCoordinate.longitude))
    // 데이터 처리는 lati, logi 값을 인자로 받아서 해당 위치의 날씨 값을 반환해주었습니다.
    WeatherDataManager.shread.setCurrentWeather(lati: lati, longi: longi) { (ok, data, error) in
      DispatchQueue.main.async {
        let currentData = data as! [String:Any]
        let imageData = try! Data(contentsOf: URL(string: "\(self.imaURL)\(currentData["icon"] as! String).png")!)
        let maxTemp = "Max: " + String((currentData["maxTemp"] as! Float).rounded()) + "°"
        let minTemp = "Min: " + String((currentData["minTemp"] as! Float).rounded()) + "°"
        self.cityLB.text = currentData["name"] as! String
        self.temperatureLB.text = "Current: " + String((currentData["temp"] as! Float).rounded()) + "°"
        self.otherTemp.text = maxTemp + "\n" + minTemp
        self.weatherImage.image = UIImage(data: imageData)
        self.indicate.stopAnimating()
        self.indicate.isHidden = true
      }
    }
  }
}
```

> 현재 위치를 알기 위해서 CoreLocation 을 사용 했습니다. CoreLocation 으로 현재위치를 가져올때, 해당 위치의 위도, 경도 값을 가지고 날씨값을 가져온 후, UI에 적용 시켜 주었습니다. 
> 

---

## City 검색 & 해당 날씨정보 적용


| * | * | 
| :--: | :--: |
|![screen](/img/posts/openWeather-1.jpg) |![screen](/img/posts/openWeather-2.jpg) | <br>

City검색에 대상이 되는 데이터는 Json 형태로 된 약 20만개정도 되는 데이터 입니다. 처음에는 20만개정도 되는 데이터를 찾는 시간이 오래걸리지 않을것 이라고 생각했는데, 검색을 해보니 UI 가 버벅거리는 현상이 생겼습니다. 버벅거리는 현상을 해결하기 위해서 검색 연산을 globalQueue에서 실행 했습니다. 정렬, 검색알고리즘을 사용하면 성능이 향상 될것 같습니다.(저의 한계는 Quick sort, Birnary Search 정도 인것 같습니다..)



```swift
** 초기 데이터 설정
func loadJsonData() {
    if let path = Bundle.main.path(forResource: "CityList", ofType: "json"), let contents = try? String(contentsOfFile: path), let data = contents.data(using: .utf8) {
      let cityDic = try! JSONSerialization.jsonObject(with: data, options: []) as! [[String: Any]]
      self.searchJsonData = []
      for city in cityDic {
        self.searchJsonData?.append(WeatherInfo(dumiData: city)!)
      }
    }
  }
** 검색 ViewController
import UIKit
class SearchViewController: UIViewController {
  @IBOutlet weak var tableView: UITableView!
  @IBOutlet weak var indicate: UIActivityIndicatorView!
  var searchController: UISearchController?
  var dataSourceOrigin: [WeatherInfo]? = []
  var searchStrArr: [String] = []
  var dataSource: [String]? = []
  
  override func viewDidLoad() {
    super.viewDidLoad()
    setNavigationItem()
    indicate.startAnimating()
    DispatchQueue.main.async {
      WeatherDataManager.shread.loadJsonData()
      self.dataSourceOrigin = WeatherDataManager.shread.searchJsonData
      for item in self.dataSourceOrigin! {
        self.searchStrArr.append(item.name)
      }
      self.indicate.hidesWhenStopped = true
      self.indicate.stopAnimating()
      print(self.dataSourceOrigin?.count)
      self.tableView.reloadData()
    }
  }
  
  func setNavigationItem() {
    self.navigationController?.navigationBar.prefersLargeTitles = true
    self.navigationItem.title = "Search"
    self.navigationItem.hidesSearchBarWhenScrolling = false
    self.navigationItem.largeTitleDisplayMode = .always
    searchController = UISearchController(searchResultsController: nil)
    self.navigationItem.searchController = searchController
    searchController?.searchResultsUpdater = self
    
    // 즉시 상호작용?
    searchController?.obscuresBackgroundDuringPresentation = false
    searchController?.searchBar.placeholder = "Search City"
    navigationItem.searchController = searchController
    definesPresentationContext = true
  }
}

extension SearchViewController: UITableViewDelegate, UITableViewDataSource{
  public func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    if searchController?.isActive == false {
      return 0
    }else {
      return dataSource?.count ?? 0
    }
  }
  
  public func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    var cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    if searchController?.isActive == false {
      return cell
    }else {
      cell.textLabel?.text = dataSource![indexPath.row]
      return cell
    }
    return cell
  }
  
  func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    let cell = tableView.cellForRow(at: indexPath)
    let cellText = cell?.textLabel?.text as! String
    let idx = searchStrArr.index(of: "\(cellText)")
    let lon = dataSourceOrigin![idx!].coord!["lon"]
    let lat = dataSourceOrigin![idx!].coord!["lat"]
    WeatherDataManager.shread.setChoiceWeather(lati: lat!, longi: lon!) { (ok, resultdata, error) in
      DispatchQueue.main.async {
        self.navigationController?.popViewController(animated: true)
      }
    }
  }
}

extension SearchViewController: UISearchResultsUpdating {
  // MARK: - UISearchResultsUpdating Delegate
  func updateSearchResults(for searchController: UISearchController) {
    let searchBar = searchController.searchBar
    if searchBar.text != nil && searchBar.text!.count > 0 {
      let item = DispatchWorkItem {[unowned self] in
        DispatchQueue.main.async {
          // 단일 검색
          if self.searchStrArr.index(of: searchBar.text!) != nil {
            let x = self.searchStrArr.index(of: searchBar.text!) // idx 넘버
            self.dataSource = [self.searchStrArr[x!]]
          }else {
            self.dataSource = []
          }
          // 연속적인 검색, 성능향상을 위한 고민이 필요함
          self.tableView.reloadData()
        }
      }
      DispatchQueue.global().async(execute: item)
    }else {
      dataSource?.removeAll()
      dataSource! += searchStrArr
    }
    tableView.reloadData()
  }
}

** 검색후 선택한 나라에 대한 날씨를 보여주는 ViewController
import UIKit
class MyChoiceWeatherTable: UIViewController {
  @IBOutlet weak var tableView: UITableView!
  let imaURL: String = "https://openweathermap.org/img/w/"
  override func viewDidLoad() {
    super.viewDidLoad()
    self.navigationController?.navigationBar.prefersLargeTitles = true
    navigationItem.title = "My Choice Weather"
    navigationItem.largeTitleDisplayMode = .always
  }
  
  override func viewDidAppear(_ animated: Bool) {
    tableView.reloadData()
  }
}

extension MyChoiceWeatherTable: UITableViewDelegate, UITableViewDataSource{
  public func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    let numberOfrow = WeatherDataManager.shread.choiceWeatherData.count
    return numberOfrow ?? 0
  }
  
  public func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    var cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    let cellText = WeatherDataManager.shread.choiceWeatherData[indexPath.row]["name"] as! String
    let cellDetailText = (WeatherDataManager.shread.choiceWeatherData[indexPath.row]["temp"] as! Float).rounded()
    let cellImageData = WeatherDataManager.shread.choiceWeatherData[indexPath.row]["icon"] as! String
    let imageData = try! Data(contentsOf: URL(string: "\(self.imaURL)\(cellImageData).png")!)
    cell.textLabel?.text = cellText
    cell.detailTextLabel?.text = String(cellDetailText)
    cell.imageView?.image = UIImage(data: imageData)
    return cell
  }
}
```

> 구조를 하나의 DataCenter 에서 모든 곳에 데이터를 뿌려주고, 추가하는 형식으로 만들었습니다.
>
> 검색의 옵션중 초성검색을 사용할때, Contains를 사용하면 초성검색이 가능하지만 조금 버벅거렸습니다. 검색 단어와, target Data를 1:1 매칭시 결과값을 반환하게 만들어 보니까, 버벅거리지는 않지만. 조금 시원하지 않은 느낌으로 마무리 지었던것 같습니다. 혹시 더 좋은 방법을 알고 계신다면, 댓글 남겨 주시면 진심으로 감사하겠습니다🙏
>
> 

---

## 여담 

Network 통신을 하면서 데이터를 핸들링 하니까. 재미도 있고, 무언가 계속 더 해보고싶은 욕망(?) 이 피어나는 것 같습니다. 주말에 카페에서 즐겁게 만들었던것 같습니다..! 감사합니다.
---




