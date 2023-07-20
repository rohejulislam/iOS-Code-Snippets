# iOS-Code-Snippets

## Manage The Keyboard

```swift
//In viewdidload(){
	//..... add bellow 2 lines in ViewDidLoad() Function 
	NotificationCenter.default.addObserver( self, selector: #selector(keyboardWillShow), name: NSNotification.Name.UIKeyboardWillShow, object: nil)
         NotificationCenter.default.addObserver( self, selector: #selector(keyboardWillHide), name: NSNotification.Name.UIKeyboardWillHide, object: nil)
//}


@objc func keyboardWillShow( notification :NSNotification ) {
        if let keyboardFrame = (notification.userInfo?[ UIKeyboardFrameEndUserInfoKey ] as? NSValue)?.cgRectValue {
            var insets: UIEdgeInsets
            insets =  UIEdgeInsetsMake( 0, 0, keyboardFrame.height, 0 )
            tableView.contentInset = insets
            tableView.scrollIndicatorInsets = insets
        }
    }
@objc func keyboardWillHide( notification :NSNotification ) {
        var insets: UIEdgeInsets
        insets = UIEdgeInsetsMake( 0, 0, 0, 0 )
        tableView.contentInset = insets
        tableView.scrollIndicatorInsets = insets
        
    }

```

## Placeholder in UITextView
```swift

extension ViewController: UITextViewDelegate {
	
	func textView(_ textView: UITextView, shouldChangeTextIn range: NSRange, replacementText text: String) -> Bool {
		
		let currentText: NSString = textView.text as NSString
		let updatedText = currentText.replacingCharacters(in: range, with:text)
		
		if updatedText.isEmpty {
			textView.text = placeholder
			textView.textColor = UIColor.lightGray
			textView.selectedTextRange = textView.textRange(from: textView.beginningOfDocument, to: textView.beginningOfDocument)
			return false
		}
		else if textView.textColor == UIColor.lightGray && !text.isEmpty {
			textView.text = nil
			textView.textColor = UIColor.black
		}
		
		return true
	}
	
	func textViewDidChangeSelection(_ textView: UITextView) {
		if self.view.window != nil {
			if textView.textColor == UIColor.lightGray {
				textView.selectedTextRange = textView.textRange(from: textView.beginningOfDocument, to: textView.beginningOfDocument)
			}
		}
	}

```

## ViewControllerMarksCodeSnippet

```swift
// MARK: - Properties
// MARK: - IBOutlets
// MARK: - Life cycle
// MARK: - Set up
// MARK: - IBActions
// MARK: - Navigation
// MARK: - Network Manager calls
// MARK: - Extensions
```


## Rounded Text field with padding 

```swift
import UIKit

@IBDesignable
class FSRoundedTextField: UITextField {
    var borderView: UIView!
    
    @IBInspectable var cornerRadius: CGFloat = 0 {
        didSet {
            layer.cornerRadius = cornerRadius
            layer.masksToBounds = cornerRadius > 0
        }
    }
    
    @IBInspectable var paddingLeft: CGFloat = 15
    @IBInspectable var paddingRight: CGFloat = 15
    
    @IBInspectable var placeholderTextColor: UIColor? {
        didSet {
            guard let attributedPlaceholder = attributedPlaceholder, let placeholderTextColor = placeholderTextColor else { return }
            self.attributedPlaceholder = NSAttributedString(string: attributedPlaceholder.string, attributes: [NSAttributedString.Key.foregroundColor: placeholderTextColor])
        }
    }
    
    @IBInspectable var localizedPlaceholderText: String? {
        didSet {
            guard let localizedPlaceholder = localizedPlaceholderText else { return }
            if let placeholderTextColor = placeholderTextColor {
                attributedPlaceholder = NSAttributedString(string: NSLocalizedString(localizedPlaceholder, comment: ""), attributes: [NSAttributedString.Key.foregroundColor: placeholderTextColor])
            } else {
                placeholder = NSLocalizedString(localizedPlaceholder, comment: "")
            }
        }
    }
    
    
    override func textRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.inset(by: UIEdgeInsets(top: 0, left: paddingLeft, bottom: 0, right: paddingRight))
    }
    
    override func editingRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.inset(by: UIEdgeInsets(top: 0, left: paddingLeft, bottom: 0, right: paddingRight))
    }
    
    override func placeholderRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.inset(by: UIEdgeInsets(top: 0, left: paddingLeft, bottom: 0, right: paddingRight))
    }
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        setup()
    }
    
    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        setup()
    }
    
    private func setup() {
        layer.cornerRadius = cornerRadius
        layer.masksToBounds = cornerRadius > 0
    }
    
}

```
