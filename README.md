# full_screen_dialog_and_overlay_swiftui

```swift
extension View {
    func fullScreenOverlay<Content: View>(
        isPresented: Binding<Bool>,
        modalTransitionStyle: UIModalTransitionStyle = .crossDissolve,
        modalPresentationStyle: UIModalPresentationStyle = .overCurrentContext,
        backgroundColor: UIColor = .clear,
        content: () -> Content
    ) -> some View {
        let keyWindow = UIApplication.keyWindow
        let vc = UIHostingController(rootView: content())
        vc.modalTransitionStyle = modalTransitionStyle
        vc.modalPresentationStyle = modalPresentationStyle
        vc.view.backgroundColor = backgroundColor
        vc.definesPresentationContext = true

        return self.onChange(of: isPresented.wrappedValue, perform: { show in
            if show {
                keyWindow?.rootViewController?.topMostViewController?.present(vc,animated: true)
            } else {
                keyWindow?.rootViewController?.topMostViewController?.dismiss(animated: true)
            }
        })
    }
}

extension View {
    func fullScreenDialog<Content: View>(
        isPresented: Binding<Bool>,
        modalTransitionStyle: UIModalTransitionStyle = .crossDissolve,
        modalPresentationStyle: UIModalPresentationStyle = .overCurrentContext,
        backgroundColor: UIColor = .clear,
        content: () -> Content
    ) -> some View{
        let keyWindow = UIApplication.keyWindow
        let vc = UIHostingController(rootView: content())
        vc.modalTransitionStyle = modalTransitionStyle
        vc.modalPresentationStyle = modalPresentationStyle
        vc.view.backgroundColor = backgroundColor
        vc.definesPresentationContext = true
        
        return self.onChange(of: isPresented.wrappedValue, perform: { show in
            if show {
                keyWindow?.rootViewController?.topMostViewController?.present(vc, animated: true)
            } else {
                keyWindow?.rootViewController?.topMostViewController?.dismiss(animated: true)
            }
        })
    }
}

extension UIApplication {
    static var keyWindow: UIWindow? {
        UIApplication
            .shared
            .connectedScenes
            .map({$0 as? UIWindowScene})
            .compactMap {$0}
            .first?
            .windows
            .filter { $0.isKeyWindow }
            .first!
    }
}


extension UIViewController {
    
    var firstWindow: UIWindow? {
        let scenes: Set<UIScene> = UIApplication.shared.connectedScenes
        let windowScene: UIWindowScene? = scenes.first as? UIWindowScene
        return windowScene?.windows.first
    }
    
    var topMostViewController: UIViewController? {
        var topMostVC = self.firstWindow?.rootViewController
        while let presentedVC = topMostVC?.presentedViewController {
            topMostVC = presentedVC
        }
        return topMostVC
    }
    
}

```
